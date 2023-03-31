# Azure 自动机器学习使用测试数据集进行验证

> 原文：<https://medium.com/analytics-vidhya/azure-automated-machine-learning-using-test-data-set-to-validate-220cafb1e4e8?source=collection_archive---------1----------------------->

# Azure 机器学习使用测试数据集来验证和组合输入和预测

# 用例

*   运行自动 ML 模式
*   使用测试数据集验证模型
*   使用单独的测试数据集，这些数据集是模型在训练中没有见过的数据
*   保存预测
*   组合输入测试数据集和预测，以生成最终的验证输出

# 密码

*   使用 python 版
*   AutoML 仅适用于此版本

```
import logging

from matplotlib import pyplot as plt
import pandas as pd
import os

import azureml.core
from azureml.core.experiment import Experiment
from azureml.core.workspace import Workspace
from azureml.core.dataset import Dataset
from azureml.train.automl import AutoMLConfigprint("This notebook was created using version 1.29 of the Azure ML SDK")
print("You are currently using version", azureml.core.VERSION, "of the Azure ML SDK")
```

*   测试时的版本是 1.28.0
*   加载订阅信息

```
ws = Workspace.from_config()

# choose a name for experiment
experiment_name = 'Titanic-automl_Test'

experiment=Experiment(ws, experiment_name)

output = {}
output['Subscription ID'] = ws.subscription_id
output['Workspace'] = ws.name
output['Resource Group'] = ws.resource_group
output['Location'] = ws.location
output['Experiment Name'] = experiment.name
pd.set_option('display.max_colwidth', -1)
outputDf = pd.DataFrame(data = output, index = [''])
outputDf.Tfrom azureml.core.compute import ComputeTarget, AmlCompute
from azureml.core.compute_target import ComputeTargetException

# Choose a name for your CPU cluster
cpu_cluster_name = "cpu-cluster"

# Verify that cluster does not exist already
try:
    compute_target = ComputeTarget(workspace=ws, name=cpu_cluster_name)
    print('Found existing cluster, use it.')
except ComputeTargetException:
    compute_config = AmlCompute.provisioning_configuration(vm_size='STANDARD_FS16_V2',
                                                           max_nodes=6)
    compute_target = ComputeTarget.create(ws, cpu_cluster_name, compute_config)

compute_target.wait_for_completion(show_output=True)# azureml-core of version 1.0.72 or higher is required
# azureml-dataprep[pandas] of version 1.1.34 or higher is required
from azureml.core import Workspace, Dataset

subscription_id = 'xxxxxxxxxxxxxxxxxxxxxxxxxxxxxx'
resource_group = 'rgname'
workspace_name = 'workspacename'

workspace = Workspace(subscription_id, resource_group, workspace_name)

dataset = Dataset.get_by_name(workspace, name='titanic_ds')
dataset.to_pandas_dataframe()
```

*   创建培训和测试数据集
*   还要配置标签列名

```
training_data, validation_data = dataset.random_split(percentage=0.8, seed=223)
label_column_name = 'Survived'automl_settings = {
    "n_cross_validations": 3,
    "primary_metric": 'average_precision_score_weighted',
    "enable_early_stopping": True,
    "max_concurrent_iterations": 2, # This is a limit for testing purpose, please increase it as per cluster size
    "experiment_timeout_hours": 0.25, # This is a time limit for testing purposes, remove it for real use cases, this will drastically limit ablity to find the best model possible
    "verbosity": logging.INFO,
}

automl_config = AutoMLConfig(task = 'classification',
                             debug_log = 'automl_errors.log',
                             compute_target = compute_target,
                             training_data = training_data,
                             label_column_name = label_column_name,

                             # Use train/test split
                             test_size=0.2,

                             **automl_settings
                            )remote_run = experiment.submit(automl_config, show_output = False)from azureml.widgets import RunDetails
RunDetails(remote_run).show()remote_run.wait_for_completion(show_output=True)
```

*   获得最佳模型

```
best_run, fitted_model = remote_run.get_output()
fitted_model
```

*   现在运行测试运行

```
test_run = next(best_run.get_children(type='automl.model_test'))
test_run.wait_for_completion(show_output=False, wait_post_processing=True)
test_run
```

*   打印测试指标

```
test_run_metrics = test_run.get_metrics()
for name, value in test_run_metrics.items():
    print(f"{name}: {value}")
```

*   查看预测的输出

```
test_run_details = test_run.get_details()
test_run_predictions = Dataset.get_by_id(ws, test_run_details['outputDatasets'][0]['identifier']['savedId'])
test_run_predictions.to_pandas_dataframe().head()
```

*   现在加载测试数据集

```
# azureml-core of version 1.0.72 or higher is required
# azureml-dataprep[pandas] of version 1.1.34 or higher is required
from azureml.core import Workspace, Dataset

subscription_id = 'c46a9435-c957-4e6c-a0f4-b9a597984773'
resource_group = 'mlops'
workspace_name = 'mlopsdev'

workspace = Workspace(subscription_id, resource_group, workspace_name)

validation_data = Dataset.get_by_name(workspace, name='titanictest')
validation_data.to_pandas_dataframe()
```

*   创建要素和标签

```
# convert the test data to dataframe
X_test_df = validation_data.drop_columns(columns=[label_column_name]).to_pandas_dataframe()
y_test_df = validation_data.keep_columns(columns=[label_column_name], validate=True).to_pandas_dataframe()
```

*   从上述模型运行预测

```
# call the predict functions on the model
y_pred = fitted_model.predict(X_test_df)
y_pred
```

*   打印混淆矩阵

```
from sklearn.metrics import confusion_matrix
import numpy as np
import itertools

cf =confusion_matrix(y_test_df.values,y_pred)
plt.imshow(cf,cmap=plt.cm.Blues,interpolation='nearest')
plt.colorbar()
plt.title('Confusion Matrix')
plt.xlabel('Predicted')
plt.ylabel('Actual')
class_labels = ['False','True']
tick_marks = np.arange(len(class_labels))
plt.xticks(tick_marks,class_labels)
plt.yticks([-0.5,0,1,1.5],['','False','True',''])
# plotting text value inside cells
thresh = cf.max() / 2.
for i,j in itertools.product(range(cf.shape[0]),range(cf.shape[1])):
    plt.text(j,i,format(cf[i,j],'d'),horizontalalignment='center',color='white' if cf[i,j] >thresh else 'black')
plt.show()
```

*   打印模型指标

```
from azureml.train.automl.model_proxy import ModelProxy

model_proxy = ModelProxy(best_run)
predictions, test_run_metrics = model_proxy.test(validation_data)

print(predictions.to_pandas_dataframe().head())
pd.DataFrame.from_dict(test_run_metrics, orient='index', columns=['Value'])
```

*   新方法
*   获得预测

```
df = validation_data.to_pandas_dataframe()predictions_df = predictions.to_pandas_dataframe()result = pd.concat(df, predictions_df, axis=1, join="inner")
```

*最初发表于*[*【https://github.com】*](https://github.com/balakreshnan/Samples2021/blob/main/AutoML/automltestds.md)*。*