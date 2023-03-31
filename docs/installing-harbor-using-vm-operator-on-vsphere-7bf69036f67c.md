# 在 vSphere 上使用 VM Operator 安装 Harbor

> 原文：<https://medium.com/analytics-vidhya/installing-harbor-using-vm-operator-on-vsphere-7bf69036f67c?source=collection_archive---------19----------------------->

![](img/efc43db43a4e08dd775ed969846e5c9a.png)

# 介绍

vSphere with Tanzu 的最新更新(v 7.0u2a)引入了一些使用 Kubernetes 管理虚拟机生命周期的激动人心的功能。这为现有工作负载的现代化创造了大量机会。作者在关于同一技术的另一篇文章中已经介绍了其中的一些模式。今天，我们将深入另一个使用 VM 操作符将容器注册中心部署为 VM 的例子。[港](https://goharbor.io/)是这里的必然选择，因为它是 VMware 提供的 Tanzu 的一部分，也是 CNCF 最强大的集装箱注册中心之一。
我们不会深入讨论如何消费 VM 操作符的细节，在其他各种博客中已经详细讨论过了。我们将直接研究如何为 DevOps 角色生成所需的清单，以便在 vSphere 环境中交付和使用稳定的注册表。

## 要求

这里的要求是部署一个企业级高可用性 Harbor registry，启用 TLS，并为其数据存储分配额外的磁盘存储。基本映像是 CentOS Stream 8 OS。

# 方法

有两个感兴趣的清单文件。第一个是在虚拟机部署的云初始化阶段需要的云初始化 YAML 文件。第二个文件是实际的虚拟机部署 YAML 文件，它使用上述云初始化 YAML 文件，并在 vSphere 上使用虚拟机操作员技术部署虚拟机。这个博客的所有相关文件都可以在[这里](https://github.com/papivot/Harbor-using-VMOperator-)找到。

## **cloud-init.yaml**

```
#cloud-config########## SECTION 1 #########
chpasswd:
    list: |
      centos:VMware1!
    expire: false
groups:
  - docker
users:
  - default
  - name: centos
    ssh-authorized-keys:
      - ssh-rsa AAAAB3....Tbfqzc8BRA3Z0YiLo+I/LIc0= nverma@bastion0
    sudo: ALL=(ALL) NOPASSWD:ALL
    groups: sudo, docker
    shell: /bin/bash########## SECTION 2 #########
network:
  version: 2
  ethernets:
      ens192:
          dhcp4: true
mounts:
  - [ /dev/sdb1, /data, "xfs", "defaults", "0", "2" ]########## SECTION 3 #########
package_update: true
yum_repos:
  docker-ce-stable:
    name: Docker CE Stable - $basearch
    baseurl: [https://download.docker.com/linux/centos/$releasever/$basearch/stable](https://download.docker.com/linux/centos/$releasever/$basearch/stable)
    enabled: true
    gpgcheck: true
    gpgkey: [https://download.docker.com/linux/centos/gpg](https://download.docker.com/linux/centos/gpg)
packages:
  - docker-ce
  - docker-ce-cli
  - containerd.io
  - net-tools
  - wget########## SECTION 4 #########
write_files: - path: /harbor/tls.pem
    content: |
      -----BEGIN CERTIFICATE-----
      MIIFNTCCBB2gAwIBAgISBOwj5VrGCSqGSIb3DQEBCwUA
      EwJSMzAeFw0yMTA0MjcwMTE0MzlaFw0E0MzlaMBQxEjA
      CW5hdmxhYi5pbzCCASIwDQYJKoZIhvEPADCCAQoCggEB
      rkRX0/k03UNmjXfpWAlxrV4ZkdjLfNeX4fr5jOOJ+wHo
      ...     
      -----END CERTIFICATE-----
    owner: 'root:root'
    permissions: '0444' - path: /harbor/tls.key
    content: |
      -----BEGIN PRIVATE KEY-----
      MIIEwAIBADANBgkqhkiG9w0BAQEFAASCQDgzn3P265EV
      NN1DZo136VgJca1eGZHYy3zXl+H6+fc2sB6FQChXgAes
      kqY3X5Bmpub3VCbxh+Zy+169BhARt4i4Lz2jqlfQ2ZxZ
      ...
      -----END PRIVATE KEY-----
    owner: 'root:root'
    permissions: '0444' - path: /harbor/install-harbor.sh
    content: |
      #!/bin/bash
      cd /harbor
      /harbor/install.sh --with-trivy --with-notary
      if [ $? -ne 0 ]
      then
        echo "Failed. Trying again"
        /harbor/install.sh --with-trivy --with-notary
      fi
    owner: 'root:root'
    permissions: '0755' - path: /harbor/harbor.yml
    content: |
      hostname: harbor-centos.navlab.io
      #http:
      #  port: 80
      https:
        port: 443
        certificate: /harbor/tls.pem
        private_key: /harbor/tls.key
      # external_url: [https://reg.mydomain.com:8433](https://reg.mydomain.com:8433)
      harbor_admin_password: VMware1!
      database:
        password: root123
        max_idle_conns: 50
        max_open_conns: 1000
      data_volume: /data
      trivy:
        ignore_unfixed: false
        skip_update: false
        insecure: false
      jobservice:
        max_job_workers: 10
      notification:
        webhook_job_max_retry: 10
      chart:
        absolute_url: disabled
      log:
        level: info
        local:
          rotate_count: 50
          rotate_size: 200M
          location: /var/log/harbor
      _version: 2.2.0
      proxy:
        http_proxy:
        https_proxy:
        no_proxy:
        components:
          - core
          - jobservice
          - trivy
    owner: root:root
    permissions: '0644'########## SECTION 5 #########
runcmd:
  - parted /dev/sdb mklabel gpt
  - parted /dev/sdb mkpart primary xfs 1MB 10240MB
  - /sbin/mkfs.xfs /dev/sdb1
  - mkdir -p /data
  - mount -t xfs --rw /dev/sdb1 /data
  - curl -L "[https://github.com/docker/compose/releases/download/1.29.1/docker-compose-$(uname](https://github.com/docker/compose/releases/download/1.29.1/docker-compose-$(uname) -s)-$(uname -m)" -o /usr/local/bin/docker-compose
  - chmod +x /usr/local/bin/docker-compose
  - ln -s /usr/local/bin/docker-compose /usr/bin/docker-compose
  - systemctl enable docker
  - systemctl start docker
  - wget -q [https://github.com/goharbor/harbor/releases/download/v2.2.1/harbor-offline-installer-v2.2.1.tgz](https://github.com/goharbor/harbor/releases/download/v2.2.1/harbor-offline-installer-v2.2.1.tgz)
  - tar --skip-old-files -xzvf harbor-offline-installer-v2.2.1.tgz
  - rm -f harbor-offline-installer-v2.2.1.tgz
  - /harbor/install-harbor.sh
```

让我们看看 YAML 档案中的一些附加注释的部分。用户可以也必须根据他们的需求修改部分。

## 第 1 部分—用户配置

用户可以根据需要修改该部分。登录(通过 SSH)虚拟机的实际要求只是为了故障排除。出于安全原因，用户可以根据环境要求自定义添加/删除该密钥和相关密钥。由于 Harbor 需要 Docker 作为一个组件来执行，因此为了进行故障排除而登录的用户应该成为 Docker 组的成员，以便进行无缝故障排除。

## 第 2 部分—系统配置(网络和磁盘)

用户可以使用此部分来提供网络和存储配置。网络配置非常简单。作者试图使用 cloud-init 特性来配置存储，但是由于一些基础映像打包问题，配置无法工作。因此，作者采取了变通办法。目前，本节中的挂载操作更新了`/etc/fstab`文件，而没有实际挂载文件系统(到`/data`)，这是当前在最后一节中执行的。

## 第 3 节—软件包安装

Harbor 要求安装 Docker，contained 及其预先要求的软件包。本节为 cloud-init 设置相关的稳定 Docker CE yum 存储库，以下载和安装适当的包。Net 工具是可选的(故障排除需要)。

## 第 4 节—生成配置文件

创建了四个文件供在云初始化阶段使用。

*   **/harbor/tls.pem** 和 **/harbor/tls.key** 是启用 SSL 和暴露 443 端口上的应用程序所需的证书文件和私钥文件。在本例中，SSL 证书中的 DNS 记录指向主机名`harbor-centos.navlab.io`(稍后将在/harbor/harbor.yml 文件中引用)

(见以下节选自[港湾网站](https://goharbor.io/docs/2.2.0/install-config/configure-https/))

> 在生产环境中，请始终使用 HTTPS。如果您启用公证内容信任来正确签署所有图像，您必须使用 HTTPS。要配置 HTTPS，您必须创建 SSL 证书。您可以使用由受信任的第三方 CA 签名的证书，也可以使用自签名证书。本节描述如何使用 [OpenSSL](https://www.openssl.org/) 来创建 CA，以及如何使用您的 CA 来签署服务器证书和客户端证书。您可以使用其他 CA 提供商，例如[让我们加密](https://letsencrypt.org/)。下面的过程假设您的 Harbor registry 的主机名是`yourdomain.com`，并且它的 DNS 记录指向您运行 Harbor 的主机。

*   **/harbor/harbor.yaml** 这是安装脚本用来安装 harbor 的实际配置文件。用户可以在这里找到详情[。注意，它引用了上面创建的 SSL 证书文件。我们需要再次强调主机名值指向 SSL 证书中的 DNS 记录。我们还将管理员密码设置为非默认值。另一个需要注意的事项是,`data_volume`值指向专门为此目的创建的新文件系统。](https://goharbor.io/docs/2.2.0/install-config/configure-yml-file/)
*   **/harbor/install-Harbor . sh**这是使用上述 harbor.yaml 文件并在启用 Trivy 和公证人的情况下在该虚拟机上安装 Harbor 的安装脚本。

## 第 5 部分—将所有内容放在一起。

这部分是奇迹发生的地方。用户可以使用前五个命令添加额外的存储，并将其挂载到/data 挂载点。这是 Harbor 存储所有数据的地方。(请注意，如果 cloud-init 磁盘创建问题得到解决，这些可能会被删除)

以下五个命令用于下载 docker-compose 的最新版本，使用正确的权限安装它并启动 docker 守护进程。

最后，我们下载最新的 Harbor 二进制文件，将它们解压到我们在第 4 节中创建所有文件的确切位置。接下来调用安装脚本来执行安装。

## vm.yaml

```
---
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: harbor-pvc
  namespace: demo1
spec:
  accessModes:
  - ReadWriteOnce
  resources:
    requests:
      storage: 10Gi
  storageClassName: nav-gold-policy
  volumeMode: Filesystem
---
apiVersion: v1
kind: ConfigMap
metadata:
    name: centos-cloudinit
    namespace: demo1
data:
  user-data: I2Nsb3VkL...5zdGFsbC1oYXJib3Iuc2gK
  hostname: harbor-centos
---
apiVersion: vmoperator.vmware.com/v1alpha1
kind: VirtualMachine
metadata:
  labels:
    vm-selector: harbor-centosvm
  name: harbor-centosvm
  namespace: demo1
spec:
  imageName: centos-stream-8-vmservice-v1alpha1-1619529007339
  className: best-effort-small
  powerState: poweredOn
  storageClass: nav-gold-policy
  networkInterfaces:
  - networkType: nsx-t
    networkName: ""
  volumes:
  - name: my-centos-vol
    persistentVolumeClaim:
      claimName: harbor-pvc
  readinessProbe:
    tcpSocket:
      port: 22
  vmMetadata:
    configMapName: centos-cloudinit
    transport: OvfEnv
---
apiVersion: vmoperator.vmware.com/v1alpha1
kind: VirtualMachineService
metadata:
  name: harbor-vmservices
spec:
  ports:
  - name: ssh
    port: 22
    protocol: TCP
    targetPort: 22
  - name: harbor
    port: 443
    protocol: TCP
    targetPort: 443
  - name: notary
    port: 4443
    protocol: TCP
    targetPort: 4443
  selector:
    vm-selector: harbor-centosvm
  type: LoadBalancer
```

该文件将在 demo1 名称空间中的 Supervisor 集群上下文中部署一个 VM。用户可以根据自己的需求修改名称空间。我们将讨论这里创建的相关对象。

## PersistantVolumeClaim

YAML 用于从存储类请求一个 10GB 的 PVC，该 PVC 已可供 vSPhere with Tanzu 中的 Supervisor 集群使用。

## **配置图**

用户需要从我们之前讨论过的 cloud-init.yaml 文件创建一个`configmap`。`user-data`是 cloud-init.yaml 文件的 base64 编码值。用户可以通过执行以下命令`cat cloud-init.yaml|base64 -w0;echo`来生成数据。将 base64 编码值复制并粘贴到 vm.yaml 文件中。vSphere `hostname`也是通过`configmap`配置的。

## 虚拟机器

用户可以使用相关的 VMclass 为 Harbor VM 提供所需的计算能力。如果这个虚拟机的`networkType`是基于`nst-t`的 WCP 环境，那么`networkName`需要留空。如果`networkType`是`vsphere-distributed`，那么需要在`networkInterfaces`部分填充相关的工作量`networkName`。

请注意，之前请求的 PVC 是作为卷安装到此虚拟机的。这将为虚拟机预设一个原始磁盘，然后通过云初始化过程对其进行格式化、分区和挂载。

## 虚拟中国服务

最后，用户通过服务类型`loadbalancer`公开 Harbor 应用程序端口 443、4443(公证)和 22(故障排除目的)

准备好这些文件后，用户可以使用`kubectl`或 CICD 进程在管理集群上下文中部署 *vm.yaml* 文件。这将部署一个功能齐全的 Harbor 虚拟机。由于在云初始化阶段完成了大量工作，虚拟机完全运行可能需要 4-5 分钟。用户可以为新创建的 VirtualMachineService 获取负载平衡器*外部 IP* 地址。一旦修改了 DNS，将这个 IP 地址指向 harbor VM 的主机名(参见 cloud-init.yaml 部分)，我们应该能够无缝地访问应用程序。

![](img/1f71bc2c43284f1143b7e38dbe2af7ec.png)