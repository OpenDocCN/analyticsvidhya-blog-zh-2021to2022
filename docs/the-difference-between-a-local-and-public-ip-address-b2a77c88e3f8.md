# 本地 IP 地址和公共 IP 地址的区别

> 原文：<https://medium.com/analytics-vidhya/the-difference-between-a-local-and-public-ip-address-b2a77c88e3f8?source=collection_archive---------8----------------------->

![](img/4f96913a09d9b3d6b2f554730be72960.png)

IP 代表互联网协议，它是设备通过互联网传输的数据格式的一套规则。一个 IP 地址，它是分配给每台设备的唯一地址，在网络上使用该协议并帮助识别这些设备。这个地址很像住宅地址，它告诉其它设备向哪里发送数据或从哪里接收数据。

IP 地址目前有两种标准。旧版本是 IPv4，允许 40 亿个唯一地址，并在地址中使用数值。由于互联网必须支持越来越多的设备，我们转向了 IPv6。现在，它通过使用十六进制数字来创建地址，支持大约 340 万亿万亿万亿个地址(3.4 x 10 38)。

# IP 地址的类型

# 静态和动态

为设备分配 IP 地址时，它可以是静态的，也可以是动态的。**静态仅仅意味着给定的地址不变。另一方面，动态 IP 是周期性变化的。** DHCP 服务器通常会分配这些地址，因为 IPv4 标准无法为现有的所有设备提供这些地址。

# 公共和私人

术语“公有”和“私有”不完全是 IP 地址的类型。相反，它们是描述网络中设备 IP 地址的标准词汇。**连接到互联网的设备有一个公共地址。未连接到互联网的设备具有私有地址，例如家庭网络或其他类型的 LAN。一个显著的区别是每个局域网通过路由器连接到互联网。**因此，公共 IP 地址被分配给路由器/调制解调器或任何用作互联网接入点的设备。

你可以通过访问像[whatismip](https://whatismyipaddress.com/)这样的网站找到你的公共 IP 地址，你的私有 IP 地址可以通过 Windows 的“ipconfig”命令和 Linux 和 Mac 的“ifconfig”来访问。

*原载于 2021 年 1 月 19 日 https://kgdavidson.co.uk**的* [*。*](https://kgdavidson.co.uk/codespertise--the-difference-between-a-local-and-public-ip-address/)