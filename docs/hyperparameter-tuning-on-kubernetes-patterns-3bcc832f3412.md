# Kubernetes 模式的超参数调谐

> 原文：<https://medium.com/analytics-vidhya/hyperparameter-tuning-on-kubernetes-patterns-3bcc832f3412?source=collection_archive---------2----------------------->

本文讨论 Kubernetes 上的超参数调优解决方案。

# 超参数调谐

如果你创建过机器学习模型，你一定遇到过超参数调优。超参数是一个模型参数，其值在学习过程开始之前就已设定。以下是一些例子:

1.  KNN 的 k
2.  决策树中的最大深度
3.  随机森林中的树木数量
4.  神经网络中的层数、每层单元数、学习速率、批量大小

更多信息，你可以参考下面的文章

[](https://www.analyticsvidhya.com/blog/2021/04/evaluating-machine-learning-models-hyperparameter-tuning/) [## 超参数调整|使用超参数调整评估 ML 模型

### 学习过程开始了。机器学习算法的关键是超参数调整。K-NN 正规化中的 K…

www.analyticsvidhya.com](https://www.analyticsvidhya.com/blog/2021/04/evaluating-machine-learning-models-hyperparameter-tuning/) [](https://towardsdatascience.com/beyond-grid-search-hypercharge-hyperparameter-tuning-for-xgboost-7c78f7a2929d) [## 超越网格搜索:XGBoost 的超荷超参数调优

### 使用 Hyperopt、Optuna 和 Ray Tune 加速机器学习超参数优化

towardsdatascience.com](https://towardsdatascience.com/beyond-grid-search-hypercharge-hyperparameter-tuning-for-xgboost-7c78f7a2929d) [](https://towardsdatascience.com/state-of-the-art-machine-learning-hyperparameter-optimization-with-optuna-a315d8564de1) [## 用 Optuna 优化最先进的机器学习超参数

### Optuna 是一个高级的超参数优化框架，具有可视化的可解释性

towardsdatascience.com](https://towardsdatascience.com/state-of-the-art-machine-learning-hyperparameter-optimization-with-optuna-a315d8564de1) 

# 超参数调整算法

如果您的机器学习模型只有一个超参数，您可以在该超参数域中尝试不同的值。然而，在现实中，一个机器学习模型可以有多个超参数，现在超参数值组合的数量迅速变得巨大。寻找最佳超参数值组合有不同的超参数调优算法:网格搜索(强力笛卡尔组合)、随机(笛卡尔组合，但随机选择)、贝叶斯(引导、知情试验)。这里有一些好文章。

[](/analytics-vidhya/comparison-of-hyperparameter-tuning-algorithms-grid-search-random-search-bayesian-optimization-5326aaef1bd1) [## 超参数调整算法的比较:网格搜索、随机搜索、贝叶斯优化

### 在模型训练阶段，模型学习其参数。但也有一些秘密旋钮，称为…

medium.com](/analytics-vidhya/comparison-of-hyperparameter-tuning-algorithms-grid-search-random-search-bayesian-optimization-5326aaef1bd1) [](https://towardsdatascience.com/intuitive-hyperparameter-optimization-grid-search-random-search-and-bayesian-search-2102dbfaf5b) [## 直观的超参数优化:网格搜索，随机搜索和贝叶斯搜索！

### 机器学习算法中的超参数就像煤气炉中的旋钮。就像我们调节煤气的旋钮一样…

towardsdatascience.com](https://towardsdatascience.com/intuitive-hyperparameter-optimization-grid-search-random-search-and-bayesian-search-2102dbfaf5b) 

# 超参数调谐组件

要将普通的机器学习训练代码转换为支持超参数调整的代码，您至少需要三样东西:

1.  超参数调优训练调用者:负责用下一个超参数值组合调用你的训练函数，收集训练函数返回的度量
2.  您的训练函数:它应该使用由超参数调整训练调用者传递的超参数值，并返回您想要优化的指标，例如精度。
3.  超参数值控制器:它使用训练函数返回的超参数值和度量来确定下一个要尝试的超参数值组合。对于网格搜索，它可能只是下一个组合，对于贝叶斯，它应该使用历史值来确定最佳方向。

要将普通的机器学习训练代码转换为支持超参数调整的代码，您需要进行三项高级更改:

1.  超参数的搜索域
2.  一个目标函数，采用超参数值组合和回报度量进行优化。此函数需要访问训练数据来训练模型，还需要访问验证数据(可以从训练数据创建)来计算模型性能指标
3.  一种优化算法。

从 Optuna 示例中截取一些概念性代码来说明模式

## 搜索域

```
classifier_name = trial.suggest_categorical("classifier", ["SVC", "RandomForest"])
svc_c = trial.suggest_float("svc_c", 1e-10, 1e10, log=True)
rf_max_depth = trial.suggest_int("rf_max_depth", 2, 32, log=True)
```

## 目标函数

```
def objective(trial):
    iris = sklearn.datasets.load_iris()
    x, y = iris.data, iris.target # parameterized hyperparameter valueclassifier_name = trial.suggest_categorical("classifier", ["SVC", "RandomForest"])
    if classifier_name == "SVC":
        svc_c = trial.suggest_float("svc_c", 1e-10, 1e10, log=True)
        classifier_obj = sklearn.svm.SVC(C=svc_c, gamma="auto")
    else:
        rf_max_depth = trial.suggest_int("rf_max_depth", 2, 32, log=True)
        classifier_obj = sklearn.ensemble.RandomForestClassifier(
            max_depth=rf_max_depth, n_estimators=10
        )score = sklearn.model_selection.cross_val_score(classifier_obj, x, y, n_jobs=-1, cv=3)
    accuracy = score.mean() **# return metric to optimize**
    return accuracy
```

## 最优化算法

```
# we want to maximize accuracy value, if the metric is loss value, usually we want to minimize
study = optuna.create_study(direction="maximize")
study.optimize(objective, n_trials=100)
print(study.best_trial)
```

然而，到目前为止，代码只在单机上运行。如果我们想使用多个节点来加速训练/超参数调优，我们可以使用容器和 Kubernetes 进行扩展。现在让我们看看 Kubernetes 上流行的超参数调优框架支持。

# 卡蒂卜

可以作为 kubeflow 的一部分安装，也可以独立安装

[](https://www.kubeflow.org/docs/components/katib/hyperparameter/#installing-katib) [## Katib 入门

### 如何设置 Katib 和执行超参数调整本指南显示了如何开始使用 Katib 和运行一些…

www.kubeflow.org](https://www.kubeflow.org/docs/components/katib/hyperparameter/#installing-katib) [](https://github.com/kubeflow/katib/blob/master/examples/v1beta1/kubeflow-training-operator/tfjob-mnist-with-summaries.yaml) [## katib/TF job-mnist-with-summarys . YAML at master kube flow/katib

### 此文件包含双向 Unicode 文本，其解释或编译可能与下面显示的不同…

github.com](https://github.com/kubeflow/katib/blob/master/examples/v1beta1/kubeflow-training-operator/tfjob-mnist-with-summaries.yaml) 

搜索域

```
parameters:
    - name: learning_rate
      parameterType: double
      feasibleSpace:
        min: "0.01"
        max: "0.05"
    - name: batch_size
      parameterType: int
      feasibleSpace:
        min: "100"
        max: "200"
  trialTemplate:
    primaryContainerName: tensorflow
    trialParameters:
      - name: learningRate
        description: Learning rate for the training model
        reference: learning_rate
      - name: batchSize
        description: Batch Size
        reference: batch_size
```

目标函数/超参数化训练函数

[](https://github.com/kubeflow/training-operator/blob/master/examples/tensorflow/mnist_with_summaries/mnist_with_summaries.py) [## 主 kubeflow/training-operator 上的 training-operator/mnist _ with _ summaries . py

### 此文件包含双向 Unicode 文本，其解释或编译可能与下面显示的不同…

github.com](https://github.com/kubeflow/training-operator/blob/master/examples/tensorflow/mnist_with_summaries/mnist_with_summaries.py) 

```
parser.add_argument('--learning_rate', type=float, default=0.001,
                      help='Initial learning rate')
  parser.add_argument('--batch_size', type=int, default=100,
                      help='Training batch size')with tf.name_scope('train'):
    train_step = tf.train.AdamOptimizer(FLAGS.learning_rate).minimize(
        cross_entropy)if train or FLAGS.fake_data:      xs, ys = mnist.train.next_batch(FLAGS.batch_size, fake_data=FLAGS.fake_data)
```

使最优化

```
# which metric top optimize
spec:
  parallelTrialCount: 3
  maxTrialCount: 12
  maxFailedTrialCount: 3
  objective:
    type: maximize
    goal: 0.99
    objectiveMetricName: accuracy_1
  algorithm:
    algorithmName: random
  metricsCollectorSpec:
    source:
      fileSystemPath:
        path: /train
        kind: Directory
    collector:
      kind: TensorFlowEvent# pass hyperparameter to training function
trialSpec:
      apiVersion: kubeflow.org/v1
      kind: TFJob
      spec:
        tfReplicaSpecs:
          Worker:
            replicas: 2
            restartPolicy: OnFailure
            template:
              spec:
                containers:
                  - name: tensorflow
                    image: gcr.io/kubeflow-ci/tf-mnist-with-summaries:1.0
                    command:
                      - "python"
                      - "/var/tf_mnist/mnist_with_summaries.py"
                      - "--log_dir=/train/metrics"
                      - "--learning_rate=${trialParameters.learningRate}"
                      - "--batch_size=${trialParameters.batchSize}"
```

# 远视

没有明显的支持 Kubernetes

# 奥普图纳

 [## 简单的并行化- Optuna 2.10.0 文档

### 编辑描述

optuna.readthedocs.io](https://optuna.readthedocs.io/en/stable/tutorial/10_key_features/004_distributed.html) 

Optuna 使用共享数据库来跟踪超参数值和指标

创建数据库

```
$ mysql -u root -e "CREATE DATABASE IF NOT EXISTS example"
$ optuna create-study --study-name "distributed-example" --storage "mysql://root@localhost/example"
[I 2020-07-21 13:43:39,642] A new study created with name: distributed-example
```

在训练代码中，创建研究并将存储指向数据库。Optuna 使用这个数据库来跟踪不同节点上的超参数分布值。

```
import optunadef objective(trial):
    x = trial.suggest_float("x", -10, 10)
    return (x - 2) ** 2if __name__ == "__main__":
    study = optuna.load_study(
        study_name="distributed-example", **storage="mysql://root@localhost/example"**
    )
    study.optimize(objective, n_trials=100)
```

更多混凝土示例

[](https://github.com/optuna/optuna-examples/tree/main/kubernetes/simple) [## optuna-examples/kubernetes/simple at main optuna/optuna-examples

### 这个示例的代码与 sklearn_simple.py 示例基本相同，除了两件事:1 -它给出了一个名称…

github.com](https://github.com/optuna/optuna-examples/tree/main/kubernetes/simple) 

在 Kubernetes 上部署数据库

```
---
apiVersion: v1
kind: Secret
metadata:
  name: postgres-secrets
type: Opaque
stringData:
  POSTGRES_PASSWORD: "superSecretPassword"
  POSTGRES_USER: "optuna"
  POSTGRES_DB: "optunaDatabase"
---
apiVersion: v1
kind: Service
metadata:
  name: postgres
spec:
  type: ClusterIP
  selector:
    app: postgres
  ports:
    - port: 5432
---
apiVersion: apps/v1
kind: StatefulSet
metadata:
  name: postgres
  labels:
    app: postgres
spec:
  selector:
    matchLabels:
      app: postgres
  serviceName: postgres
  template:
    metadata:
      labels:
        app: postgres
    spec:
      containers:
        - name: postgres
          image: postgres:latest
          imagePullPolicy: IfNotPresent
          envFrom:
            - secretRef:
                name: postgres-secrets
          ports:
            - containerPort: 5432
---
```

提交多节点超参数调整

```
apiVersion: batch/v1
kind: Job
metadata:
  name: study-creator
spec:
  template:
    spec:
      restartPolicy: OnFailure
      initContainers:
        - name: wait-for-database
          image: postgres:latest
          imagePullPolicy: IfNotPresent
          command:
          - /bin/sh
          - -c
          - -e
          - -x
          - |
            until pg_isready -U $(POSTGRES_USER) -h postgres -p 5432;
            do echo "waiting for postgres"; sleep 2; done;
          envFrom:
            - secretRef:
                name: postgres-secrets
      containers:
        - name: study-creator
          image: optuna-kubernetes:example
          imagePullPolicy: IfNotPresent
          command:
          # create study **- /bin/sh
          - -c
          - -e
          - -x
          - |
            optuna create-study --skip-if-exists --direction maximize \
            --study-name "kubernetes" --storage \
            "postgresql://${POSTGRES_USER}:${POSTGRES_PASSWORD}**[**@postgres**](http://twitter.com/postgres)**:5432/${POSTGRES_DB}"**
          envFrom:
            - secretRef:
                name: postgres-secrets
---
apiVersion: batch/v1
kind: Job
metadata:
  name: worker
spec:
  parallelism: 5
  template:
    spec:
      restartPolicy: OnFailure
      initContainers:
        - name: wait-for-study
          image: optuna-kubernetes:example
          imagePullPolicy: IfNotPresent
          command:
 **- /bin/sh
          - -c
          - -e
          - -x
          - |
            until [ `sh check_study.sh` -eq 0 ];**
            do echo "waiting for study"; sleep 2; done;
          envFrom:
            - secretRef:
                name: postgres-secrets
      containers:
        - name: worker
          image: optuna-kubernetes:example
          imagePullPolicy: IfNotPresent
          command:
 **- python
            - sklearn_distributed.py**
          envFrom:
            - secretRef:
                name: postgres-secrets
---
```

指向 Postgres 的存储

```
if __name__ == "__main__":
    study = optuna.load_study(
        study_name="kubernetes",
        storage="postgresql://{}:{}[@postgres](http://twitter.com/postgres):5432/{}".format(
            os.environ["POSTGRES_USER"],
            os.environ["POSTGRES_PASSWORD"],
            os.environ["POSTGRES_DB"],
        ),
    )
    study.optimize(objective, n_trials=20)
    print(study.best_trial)
```

# 射线调谐

您可以在 Kubernetes 上部署 Ray Kubernetes operator/cluster，它包含 head 和 worker。

 [## 雷 1.8.0 版

### 您可以利用 Kubernetes 集群作为执行分布式 Ray 程序的基础。射线自动缩放器…

docs.ray.io](https://docs.ray.io/en/latest/cluster/kubernetes.html) 

光线调节支持许多超参数调节库，包括 Optuna，hyperopt。

[](https://docs.ray.io/en/latest/tune/api_docs/suggestion.html) [## 雷 1.8.0 版

### Tune 的搜索算法是围绕开源优化库的包装器，用于高效的超参数选择…

docs.ray.io](https://docs.ray.io/en/latest/tune/api_docs/suggestion.html) [](/optuna/scaling-up-optuna-with-ray-tune-88f6ca87b8c7) [## 使用光线调节放大 Optuna

### 作者:凯·弗里克，克里斯曼·卢米斯，理查德·廖

medium.com](/optuna/scaling-up-optuna-with-ray-tune-88f6ca87b8c7) 

# 附录

[](https://neptune.ai/blog/optuna-vs-hyperopt) [## Optuna vs Hyperopt:应该选择哪个超参数优化库？- neptune.ai

### 思考应该选择哪个库进行超参数优化？使用远视有一段时间了，感觉像…

海王星. ai](https://neptune.ai/blog/optuna-vs-hyperopt) [](https://towardsdatascience.com/tuning-hyperparameters-with-optuna-af342facc549) [## 使用 Optuna 调整超参数

### 以自动和智能的方式调整机器学习模型的超参数

towardsdatascience.com](https://towardsdatascience.com/tuning-hyperparameters-with-optuna-af342facc549) [](https://towardsdatascience.com/optuna-a-flexible-efficient-and-scalable-hyperparameter-optimization-framework-d26bc7a23fff) [## OPTUNA:一个灵活、高效、可扩展的超参数优化框架

### 轻型和大型超参数优化的新选择

towardsdatascience.com](https://towardsdatascience.com/optuna-a-flexible-efficient-and-scalable-hyperparameter-optimization-framework-d26bc7a23fff) [](https://towardsdatascience.com/how-to-make-your-model-awesome-with-optuna-b56d490368af) [## 如何用 Optuna 让你的模型牛逼

### 轻松高效地优化模型的超参数

towardsdatascience.com](https://towardsdatascience.com/how-to-make-your-model-awesome-with-optuna-b56d490368af)