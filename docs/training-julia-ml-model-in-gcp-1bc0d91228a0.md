# 在 GCP 培训 Julia ML 模型

> 原文：<https://medium.com/analytics-vidhya/training-julia-ml-model-in-gcp-1bc0d91228a0?source=collection_archive---------21----------------------->

尽管 Google Cloud AI 平台应该运行 Python 模块中的 ML 模块，但我使用了一个技巧，通过将 Julia ML 模块打包在 Docker 映像中，在 GCP AI 平台上运行 Julia 训练模块。这

1.  创建 [*Dockerfile*](https://github.com/iwasnothing/JuliaConvGRU/blob/main/Dockerfile)

```
FROM julia
WORKDIR /app
COPY *.jl /app/
COPY OsRSIConv/ /app/OsRSIConv/
RUN julia installPkg.jlENTRYPOINT ["julia", "run.jl"]
```

2.构建 docker 映像并将其推送到 GCP 映像库。

```
docker build -t gar.io/<project>/osrsi .
docker push gcr.io/<project>/osrsi
```

3.参照参数: *master-image-uri 中的图片，将 docker 图片作为训练作业提交给 GCP AI 平台。*

```
gcloud ai-platform jobs submit training $job_id \
    --region "us-central1" \
    --master-image-uri=gcr.io/$PROJECT_ID/osrsi:latest
```

没有必要创建任何渴望虚拟机，GCP 只收取你的工作所使用的 CPU 信用。

包括模型文件、csv 输出在内的任何输出都可以存储在 GCP 存储器中。虽然 Julia 有第三方 [GCP 存储 API，但是使用`run`函数运行 shell 命令`gsutil`要容易得多。](https://juliacloud.github.io/GoogleCloud.jl/latest/)

```
run(`google-cloud-sdk/bin/gsutil cp file.csv gs://bucket/file.csv`)
```

为了在 shell 中运行 google-cloud-sdk，应该为 sdk 和 python 安装修改上面的 order 文件。这是要添加的一行。

```
RUN apt-get update && apt-get install -y curl && apt-get install wget -y
RUN curl -O [https://dl.google.com/dl/cloudsdk/channels/rapid/downloads/google-cloud-sdk-324.0.0-linux-x86_64.tar.gz](https://dl.google.com/dl/cloudsdk/channels/rapid/downloads/google-cloud-sdk-324.0.0-linux-x86_64.tar.gz)
RUN gzip -cd google-cloud-sdk-324.0.0-linux-x86_64.tar.gz|tar -xvf -
RUN wget [https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh](https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh)
RUN bash Miniconda3-latest-Linux-x86_64.sh -b -p /app/miniconda
ENV PATH $PATH:/app/google-cloud-sdk/bin:/app/miniconda/bin
```

# CI/CD 管道

使用 GCP 云构建来构建自动化 docker 构建和培训管道。

1.  为 GitHub repo 创建构建触发器。
2.  触发器将引用自定义的 clouldbuild.yaml 来构建 docker 映像并提交培训作业。

[*cloud build . YAML*](https://github.com/iwasnothing/JuliaConvGRU/blob/main/cloudbuild.yaml)

```
steps:- name: 'gcr.io/cloud-builders/docker'
  args: ['build','-t','gcr.io/$PROJECT_ID/osrsi','.']
  timeout: 3600s
- name: 'gcr.io/cloud-builders/docker'
  args: [ 'push', 'gcr.io/$PROJECT_ID/osrsi' ]
  timeout: 3600s
- name: 'gcr.io/cloud-builders/gcloud'
  entrypoint: 'bash'
  args:
    - '-eEuo'
    - 'pipefail'
    - '-c'
    - |-
    ts=`date +%Y%m%d%H%M`
    job_id="julia_osrsi_stock_training_$ts"
    gcloud ai-platform jobs submit training $job_id \
    --region "us-central1" \
    --master-image-uri=gcr.io/$PROJECT_ID/osrsi:latesttimeout: 3600s
```

3.对主分支的任何推送都将触发构建，并在 AI 平台上自动运行作业。

4.(可选)通过使用以下 trigger REST API，使用 Cloud scheduler 定期运行云构建触发器，比如每天运行一次:

```
[https://cloudbuild.googleapis.com/v1/projects/<project>/triggers/<trigger-id>:run](https://cloudbuild.googleapis.com/v1/projects/iwasnothing-self-learning/triggers/61e9fa72-030d-4a12-b4ed-f346dc874e70:run)
```