# 时间序列预测模型的 AWS 阶跃函数推理管道

> 原文：<https://medium.com/analytics-vidhya/aws-step-function-inference-pipeline-for-time-series-forecasting-model-9d5cc3121f88?source=collection_archive---------11----------------------->

在之前的[文章](/@tanaychowdhury/aws-step-function-pipeline-for-time-series-forecasting-model-using-tensorflow-d993abc10ddc)中，我介绍了我们如何创建一个训练管道来训练时间序列数据上的 Tensorflow 模型，并由此创建一个 SageMaker 端点。在本文中，我将解释如何通过端到端管道调用端点。

AWS 步骤函数管道如下所示。在这个管道步骤中，函数通过调用 AWS s3、AWS Athena、AWS SageMaker 和 AWS Lambda 服务来协调工作流。