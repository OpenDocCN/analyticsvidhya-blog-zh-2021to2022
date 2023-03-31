# 将 Kubernetes 从 Docker 迁移到 Containerd

> 原文：<https://medium.com/analytics-vidhya/migrate-kubernetes-from-docker-to-containerd-2fba80779280?source=collection_archive---------4----------------------->

![](img/b4c75e330d0a4bb0076093b9d24ccd89.png)

由[卡梅伦·文蒂](https://unsplash.com/@ventiviews?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](https://unsplash.com/s/photos/shipping-container?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄的照片

正如大多数人可能知道的，Kubernetes 决定在 **v1.20** 之后不再使用 Docker 作为容器运行时

关于 Kubernetes 为什么决定弃用 Docker 的解释可以在这里找到:[https://Kubernetes . io/blog/2020/12/02/don-panic-Kubernetes-and-Docker/](https://kubernetes.io/blog/2020/12/02/dont-panic-kubernetes-and-docker/)

这对我们终端用户来说意味着，无论如何，我们都必须迁移到另一个容器运行时。有几个容器运行时接口，但可能众所周知的有:

*   [集装箱](https://github.com/containerd)
*   [cri-o](https://cri-o.io/)

在本文中，我们将把 Kubernetes 集群从 Docker 迁移到 Containerd。这些更改将应用到群集中的所有节点。我建议从`worker`节点开始迁移。

我将迁移我的操场集群，它有 3 个主节点和 3 个工作节点，运行时使用 Docker

```
NAME           STATUS   ROLES                  AGE   VERSION   INTERNAL-IP    EXTERNAL-IP   OS-IMAGE             KERNEL-VERSION     CONTAINER-RUNTIME
k8s-master-1   Ready    control-plane,master   xxd   v1.20.6   xxx.xxx.x.xx   <none>        Ubuntu 20.04.x LTS   x.x.x-xx-generic   docker://19.03.6
k8s-master-2   Ready    control-plane,master   xxd   v1.20.6   xxx.xxx.x.xx   <none>        Ubuntu 20.04.x LTS   x.x.x-xx-generic   docker://19.03.6
k8s-master-3   Ready    control-plane,master   xxd   v1.20.6   xxx.xxx.x.xx   <none>        Ubuntu 20.04.x LTS   x.x.x-xx-generic   docker://19.03.6
k8s-worker-1   Ready    worker                 xxd   v1.20.6   xxx.xxx.x.xx   <none>        Ubuntu 20.04.x LTS   x.x.x-xx-generic   docker://19.03.6
k8s-worker-2   Ready    worker                 xxd   v1.20.6   xxx.xxx.x.xx   <none>        Ubuntu 20.04.x LTS   x.x.x-xx-generic   docker://19.03.6
k8s-worker-3   Ready    worker                 xxd   v1.20.6   xxx.xxx.x.xx   <none>        Ubuntu 20.04.x LTS   x.x.x-xx-generic   docker://19.03.6
```

# 准备节点

首先，必须禁用调度，并且必须清除所有不必要的工作负载，守护进程集除外。

从封锁节点开始

```
kubectl cordon k8s-worker-3
node/k8s-worker-3 cordoned
```

漏极节点

```
kubectl drain k8s-worker-3 --ignore-daemonsets --delete-emptydir-data
node/k8s-worker-3 already cordoned
<...>
node/k8s-worker-3 evicted
```

阻止库伯莱特和多克

```
systemctl stop kubelet
systemctl stop docker
```

# 切换到容器 d

节点已准备好迁移到 containerd。从移除 docker 开始，因为不再需要它了

```
apt purge docker-ce docker-ce-cli
```

确保安装了`containerd`:

```
ctr -n moby container list
```

确保`/etc/containerd/config.toml`中容器 id 的配置文件存在。您可以通过以下方式生成它

```
sudo mkdir -p /etc/containerd
containerd config default | sudo tee /etc/containerd/config.toml
```

如果使用 systemd 作为 cgroup 驱动程序，必须在`containerd` config 中进行配置。在`/etc/containerd/config.toml`添加

```
<...>
[plugins."io.containerd.grpc.v1.cri".containerd.runtimes.runc]
  <...>
  [plugins."io.containerd.grpc.v1.cri".containerd.runtimes.runc.options]
    SystemdCgroup = true # <--- This line
<...>
```

重新启动容器 d

```
systemctl restart containerd
```

通过添加容器运行时标志来编辑`/var/lib/kubelet/kubeadm-flags.env`文件

*   `--container-runtime=remote`
*   `--container-runtime-endpoint=unix:///run/containerd/containerd.sock`

`kubeadm-flags.env`文件现在应该是这样的:

```
cat /var/lib/kubelet/kubeadm-flags.env 
KUBELET_KUBEADM_ARGS="--network-plugin=cni --pod-infra-container-image=k8s.gcr.io/pause:3.2 --container-runtime=remote --container-runtime-endpoint=unix:///run/containerd/containerd.sock"
```

启动 Kubelet

```
systemctl start kubelet
```

检查集群状态

```
NAME           STATUS                      ROLES                  AGE   VERSION   INTERNAL-IP    EXTERNAL-IP   OS-IMAGE             KERNEL-VERSION     CONTAINER-RUNTIME
k8s-master-1   Ready                       control-plane,master   xxd   v1.20.6   xxx.xxx.x.xx   <none>        Ubuntu 20.04.x LTS   x.x.x-xx-generic   docker://19.03.6
k8s-master-2   Ready                       control-plane,master   xxd   v1.20.6   xxx.xxx.x.xx   <none>        Ubuntu 20.04.x LTS   x.x.x-xx-generic   docker://19.03.6
k8s-master-3   Ready                       control-plane,master   xxd   v1.20.6   xxx.xxx.x.xx   <none>        Ubuntu 20.04.x LTS   x.x.x-xx-generic   docker://19.03.6
k8s-worker-1   Ready                       worker                 xxd   v1.20.6   xxx.xxx.x.xx   <none>        Ubuntu 20.04.x LTS   x.x.x-xx-generic   docker://19.03.6
k8s-worker-2   Ready                       worker                 xxd   v1.20.6   xxx.xxx.x.xx   <none>        Ubuntu 20.04.x LTS   x.x.x-xx-generic   docker://19.03.6
k8s-worker-3   Ready,SchedulingDisabled    worker                 xxd   v1.20.6   xxx.xxx.x.xx   <none>        Ubuntu 20.04.x LTS   x.x.x-xx-generic   containerd://1.4.4
```

正如你所看到的，这个工人开始使用`containerd`作为它的运行时。检查`daemonsets`是否运行正常。`Uncordon`一切看起来都很好的节点

```
kubectl uncordon k8s-worker-3
```

对所有节点重复该过程(一个接一个)

# 迁移后

祝贺您将集群迁移到`containerd`。然而，要完全迁移，剩下的事情很少。让我们通过删除 docker 相关的文件夹来释放一些空间。不再需要它们了

```
rm -r /etc/docker
rm -r /var/lib/docker
rm -r /var/lib/dockershim
```

如果您使用`kubeadm`来管理集群初始化、加入和更新，您可能想要重新注释节点，这样在您下次更新时`kubeadm`就不会混淆您是使用`docker`还是`containterd`作为运行时:

```
kubectl annotate node k8s-worker-3 --overwrite kubeadm.alpha.kubernetes.io/cri-socket=unix:///run/containerd/containerd.sock
```

请注意，在将整个集群迁移到`containerd`并确保迁移成功后，必须在所有节点上执行删除和注释。