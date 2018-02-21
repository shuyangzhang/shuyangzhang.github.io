---
layout:     post
title:      "使用VPS搭建SS服务器频繁timed out的解决办法"
subtitle:   " \"shadowsocks真乃神器也！\""
date:       2018-02-20 17:24:00
author:     "Zhang Shuyang"
header-img: "img/post-bg-2015.jpg"
tags:
    - shadowsocks
    - vps
    - iptables
---

#### 写在前面
这篇文章是我2017年2月21日发表在新浪博客上的，此处仅为重发搬运。（新浪博客上共有三篇文章，这是第三篇，之后的文章都会直接发表在这里）

#### 问题的原因
如果VPS选择的是CentOS 7.0以上的版本一般是因为centos防火墙默认对其他端口的规则导致的，iptables的默认规则是所有端口都关闭，因此才会出现频繁timed out的情况。

#### 解决方案
只需将SS的端口在防火墙中设置打开即可，代码如下（以6022端口为例）

`$ firewall-cmd --zone=public --add-port=6022/tcp --permanent`  
`$ firewall-cmd --zone=public --add-port=6022/udp --permanent`  
`$ firewall-cmd --complete-reload`  

当然也可以直接把防火墙的所有规则都删掉，但是这显然是不安全的，不推荐这么做  

`$ iptables -F`  


