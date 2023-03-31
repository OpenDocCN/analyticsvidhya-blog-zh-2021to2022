# Azure ML — ParallelRunStep —按比例运行批处理分数

> 原文：<https://medium.com/analytics-vidhya/azure-ml-parallelrunstep-run-batch-score-in-scale-fed169b08908?source=collection_archive---------2----------------------->

# 如何大规模并行运行批量培训

# 用例

*   并行运行模型进行训练
*   基于数据大小进行扩展

# 密码

*   首先创建一个存储帐户
*   创建一个名为泰坦尼克号的目录
*   从数据文件夹上传至少 2 个 Tiantic.csv。对于第二个文件，复制并重命名为 Titanic1.csv
*   现在创建一个笔记本

```
from azureml.core import Workspace
ws = Workspace.from_config()from azureml.core import Workspace, Datasetsubscription_id = 'xxxxxxxxxxxxxxxxxxxxx'
resource_group = 'xxxxxxxxxxxxxxx'
workspace_name = 'xxxxxxxxxxxxxxxx'workspace = Workspace(subscription_id, resource_group, workspace_name)dataset = Dataset.get_by_name(workspace, name='TitanicTraining')
data = dataset.to_pandas_dataframe()
```

*   现在创建一个数据存储
*   这将访问 blob 存储
*   创建一个数据存储“titanicstore”

```
from azureml.core.datastore import Datastoreaccount_name = "storageacctname"
datastore_name="titanicstore"
container_name="titanic"titanic_data = Datastore.register_azure_blob_container(ws, 
                      datastore_name=datastore_name, 
                      container_name= container_name, 
                      account_name=account_name, 
                      overwrite=True)
```

*   从上述数据存储创建数据集

```
from azureml.core.dataset import Datasettitanic_ds_name = 'titanic_data'path_on_datastore = titanic_data.path('/')
input_titanic_ds = Dataset.Tabular.from_delimited_files(path=path_on_datastore, validate=False)
named_titanic_ds = input_titanic_ds.as_named_input(titanic_ds_name)
```

*   设置输出文件夹配置

```
from azureml.core.dataset import Dataset
from azureml.data import OutputFileDatasetConfigoutput_dir = OutputFileDatasetConfig(name="scores")
```

*   创建要运行的计算群集

```
from azureml.core.compute import AmlCompute, ComputeTarget
from azureml.exceptions import ComputeTargetException
compute_name = "cpu-cluster"# checks to see if compute target already exists in workspace, else create it
try:
    compute_target = ComputeTarget(workspace=ws, name=compute_name)
except ComputeTargetException:
    config = AmlCompute.provisioning_configuration(vm_size="Standard_F16s_v2",
                                                   vm_priority="dedicated", 
                                                   min_nodes=0, 
                                                   max_nodes=4) compute_target = ComputeTarget.create(workspace=ws, name=compute_name, provisioning_configuration=config)
    compute_target.wait_for_completion(show_output=True, min_node_count=None, timeout_in_minutes=20)
```

*   编写批量训练代码

```
%%writefile batch_scoring.py
# azureml-core of version 1.0.72 or higher is required
# azureml-dataprep[pandas] of version 1.1.34 or higher is required
import io
import pickle
import argparse
import numpy as np
import pandas as pdimport joblib
import os
import urllib
import shutil
import azuremlfrom azureml.core.model import Model
from azureml.core import Workspace, Dataset #model_path = "model_08102021.pkl"#model = joblib.load(model_path)#data = dataset.to_pandas_dataframe().drop(columns="Survived")#result = model.predict(data)
def init():
    global test_model model_path = Model.get_model_path("titanic") #model_path = Model.get_model_path(args.model_name)
    with open(model_path, 'rb') as model_file:
        test_model = joblib.load(model_file)def run(mini_batch):
    # Load the dataset
    subscription_id = 'xxxxxxxxxxxxxxxxxxxxxxxx'
    resource_group = 'rgname'
    workspace_name = 'workspacename' workspace = Workspace(subscription_id, resource_group, workspace_name) dataset = Dataset.get_by_name(workspace, name='TitanicTraining')
    dataset.to_pandas_dataframe() # make inference    
    print(f'run method start: {__file__}, run({mini_batch})')
    resultList = []
    for file in mini_batch:
        input_data = dataset.to_pandas_dataframe().drop(columns="Survived")
        pred = test_model.predict(input_data) # cleanup output
    #result = input_data.drop(input_data.columns[4:], axis=1)
    result = input_data
    result['PredOutput'] = pred return result
```

*   环境

```
from azureml.core import Environment
from azureml.core.conda_dependencies import CondaDependencies
from azureml.core.runconfig import DEFAULT_CPU_IMAGEcd = CondaDependencies.create(pip_packages=["azureml-train-automl-runtime==1.32.0","inference-schema","azureml-interpret==1.32.0","azureml-defaults==1.32.0", "numpy>=1.16.0,<1.19.0",
"pandas==0.25.1","scikit-learn==0.22.1", "fbprophet==0.5","holidays==0.9.11","psutil>=5.2.2,<6.0.0", "xgboost<=0.90"])env = Environment(name="parallelenv")
env.python.conda_dependencies = cd
env.docker.base_image = DEFAULT_CPU_IMAGE
```

*   并行配置的配置

```
from azureml.pipeline.steps import ParallelRunConfigparallel_run_config = ParallelRunConfig(
    environment=env,
    entry_script="batch_scoring.py",
    #source_directory=".",
    output_action="append_row",
    mini_batch_size="1MB",
    error_threshold=1,
    allowed_failed_count=2,
    compute_target=compute_target,
    process_count_per_node=16,
    node_count=4
)from azureml.pipeline.steps import ParallelRunStep
from datetime import datetimeparallel_step_name = "batchscoring-" + datetime.now().strftime("%Y%m%d%H%M")label_config = Dataset.get_by_name(workspace, name='TitanicTraining')batch_score_step = ParallelRunStep(
    name=parallel_step_name,
    inputs=[named_titanic_ds],
    #inputs=[label_ds],
    output=output_dir,
    #arguments=["--model_name", "inception",
    #           "--labels_dir", label_config],
    #side_inputs=[label_config],
    parallel_run_config=parallel_run_config,
    allow_reuse=False
)from azureml.core import Experiment
from azureml.pipeline.core import Pipelinepipeline = Pipeline(workspace=ws, steps=[batch_score_step])
pipeline_run = Experiment(ws, 'Tutorial-Batch-Scoring').submit(pipeline)
pipeline_run.wait_for_completion(show_output=True)
```

*   下载结果并显示

```
import pandas as pdbatch_run = next(pipeline_run.get_children())
batch_output = batch_run.get_output_data("scores")
batch_output.download(local_path="inception_results")for root, dirs, files in os.walk("inception_results"):
    for file in files:
        if file.endswith("parallel_run_step.txt"):
            result_file = os.path.join(root, file)df = pd.read_csv(result_file, delimiter=":", header=None)
#df.columns = ["Filename", "Prediction"]
print("Prediction has ", df.shape[0], " rows")
df.head(10)
```

*最初发表于*[*【https://github.com】*](https://github.com/balakreshnan/Samples2021/blob/main/DataScience/ParallelTabularScoring.md)*。*