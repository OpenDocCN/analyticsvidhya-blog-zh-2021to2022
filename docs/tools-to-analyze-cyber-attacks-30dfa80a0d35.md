# 分析网络攻击的工具

> 原文：<https://medium.com/analytics-vidhya/tools-to-analyze-cyber-attacks-30dfa80a0d35?source=collection_archive---------7----------------------->

![](img/6d33960d59b92221a5c1bfea2c86af93.png)

本文涵盖了我们可以用来分析网络攻击的工具。这些工具和技术对每种类型的攻击都有不同的使用案例。

***侦察***

这是一个收集域或 IP 地址信息的过程。这是最常用的了解域，子域，服务器位置等，这样我们就可以知道他们的弱点，并利用他们进入系统。

*   [](https://www.whois.com/whois/)**—是域名或 IP 解析器**

*   **[**netcraft.com**](https://sitereport.netcraft.com/)**—添加了类似 DMARC/站点技术的信息以及域名/URL 分析****
*   ****[**Wappalyzer**](https://www.wappalyzer.com/)**——这是一个 chrome 扩展，我们可以将它作为一个扩展添加到我们的网络浏览器中，以找到关于网站的技术信息******
*   ********Crimeflare.org**—一些网站特别托管在 cloudflare 上，因此它专门定制来查找网站通过 cloudflare 托管的隐藏子域。******

*******流行工具*******

*   ****[**牛肉工具**](https://beefproject.com/) —浏览器开发框架。它是一个专注于网络浏览器的渗透测试工具****
*   ****[**shod an Search**](https://www.shodan.io/)—shod an 是一个搜索引擎，可用于查找您设备的安全漏洞****
*   ****[**Google Hacking 或 Google Dorks**](https://www.exploit-db.com/google-hacking-database)—Google Hacking 也称为 Google dorking，是一种黑客技术，它使用谷歌搜索和其他谷歌应用程序来寻找网站正在使用的配置和计算机代码中的安全漏洞。****
*   ****[**DNS 查找**](https://mxtoolbox.com/DNSLookup.aspx) — 该测试将按优先级顺序列出一个域的 DNS 记录。DNS 查找是直接针对域的权威名称服务器进行的，因此对 DNS 记录的更改应该会立即显示出来。默认情况下，DNS 查找工具将返回 IP 地址****
*   ****[**NSLookup**](https://www.nslookup.io/)—NSLookup 是一个网络管理命令行工具，用于查询域名系统以获取域名和 IP 地址之间的映射或其他 DNS 记录****
*   ****[**DIG(域名信息搜索工具)**](https://toolbox.googleapps.com/apps/dig/) — DIG 是一个用于查询域名系统的网络管理命令行工具。这对于网络故障排除和教学非常有用。****
*   ****[**马鹿安全**](https://wapiti.sourceforge.io/)—web 应用漏洞扫描器。Wapiti 允许您审计您的网站或网络应用程序的安全性****
*   ****[**凶猛**](https://github.com/mschwager/fierce) —凶猛是一种半轻量级扫描仪，有助于定位非连续。指定域的 IP 空间和主机名。****
*   ****[**Edgar**](https://www.sec.gov/edgar/search-and-access) —电子数据收集、分析和检索—是美国证券交易委员会创建的电子归档系统，旨在提高企业归档的效率和可访问性。所有上市公司在向美国证券交易委员会提交所需文件时都会使用该系统****
*   ****[**Wireshark**](https://www.wireshark.org/)**—**Wireshark 是一款免费的开源包分析器。它用于网络故障排除、分析、软件和通信协议开发。****
*   ****[**HTTrack**](https://www.httrack.com/)—HTTrack 是一款免费易用的离线浏览器实用程序。它允许你从因特网下载一个万维网站点到一个本地目录，递归地构建所有目录，从服务器获取 HTML、图像和其他文件到你的计算机。****
*   ******Robot.txt 文件** —文件机器人。txt 用于向 web 机器人(如搜索引擎爬虫)发出指令，指示机器人可以或不可以在网站中的哪些位置进行爬行和索引。****
*   ****[**Nmap**](https://nmap.org/)**—**Nmap 用于通过发送数据包和分析响应来发现计算机网络上的主机和服务。Nmap 为探测计算机网络提供了许多功能，包括主机发现和服务以及操作系统检测****

****这些工具将帮助我们找到攻击系统的入侵者。这对于那些以 Bug Bounty 或者作为服务器管理员或分析师开始职业生涯的人来说很有用。****