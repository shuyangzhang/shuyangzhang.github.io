---
layout:     post
title:      "使用letsencrypt给网站增加https证书"
subtitle:   " \"已经2018年了，你还没使用https吗？\""
date:       2018-02-20 16:41:00
author:     "Zhang Shuyang"
header-img: "img/post-bg-2015.jpg"
tags:
    - https
    - nginx
    - LNMP
    - linux
---


####  写在最前

这篇文章是我2016年12月12日发表在新浪博客上的，此处仅为重发搬运。（新浪博客上共有三篇文章，这是第二篇）

#### 前言

最近刚刚租了aliyun服务器，第一次配置LNMP环境，遇到了很多关于nginx配置方面的问题（如为网站增加https证书），现在将解决这些问题时踩的一些坑和解决方法记录下来，一方面方便自己日后查阅，另一方面能给后来者一些参考。

#### HTTPS的优势

使用HTTPS加密之后不仅能避免无良运营商对ISP的劫持，也能对传输内容进行加密，从某种层面来讲也能避免网络审查或被监听后的安全问题。

**以下是我的朋友awabimakoto对http与https的理解，仅供参考**

> http明文的话，中间人改了你也不知道。
> https密文的话，他就得伪造证书，这样浏览器会提示不受信任。(比如12306)
> 可以制造自签名的证书，但没有权威机构认证，会不受信任。权威机构的密钥内置在浏览器中。
> 目前提供免费证书的权威机构只有startcom和letsencrypt。（据我所知）可是startcom被中国沃通收购后名声一败涂地。
> 自签名的证书可以测试用，用于生产是不好的。虽然偏偏有人这么干。(比如12306)
> 关于网络审查方面，https避免了关键字过滤的审查方式。但是https并不能避免dns污染 ip封锁 ddos攻击等问题。

#### HTTPS证书的选择

提供证书的服务商有很多，大多都是收费的，我采用的是一家免费且靠谱的证书机构letsencrypt，官网：https://letsencrypt.org/ 证书免费使用三个月，快要到期的时候可以延长使用时间并且依然免费。
该证书支持的浏览器有android2.3.6以上，firefox2.0以上，windows vista以上，ios3.1以上，chrome全平台。那么言外之意就是放弃了xp和ie7以下的用户，个人看来完全没问题。

#### 流程
分为三步：**1.安装certbot  2.创建配置文件  3.让certbot自动更新证书**

**1.安装certbot**

certbot官网为：https://certbot.eff.org/  需要选择你的操作系统和http服务器，我这里用的是centos7.2+nginx，选择了对应的选项后会有匹配的指引参考：
以下均以redhat系的yum包管理器为例

`$ sudo yum install certbot`

**2.1创建配置文件**

`$ sudo certbot certonly`
随后是一段交互式的操作，无非就是输入域名和nginx里配置的wwwroot，随后该脚本会自动生成一个页面并检测该域名是否属于你，如果验证成功会自动生成一个目录和多个配置文件，目录为 /etc/letsencrypt/live/example.com/  生成的文件分别为 cert.pem privkey.pem chain.pem fullchain.pem

**2.2配置nginx**

利用mozilla提供的一个nginx配置文件自动生成工具可以快速生成配置文件，只需要改动部分内容即可，地址为：https://mozilla.github.io/server-side-tls/ssl-config-generator/  按照我的配置选择 Nginx+intermediate+server version 1.10.1 +openssl version 1.0.1e   
随后仅仅需要修改几处配置文件即可

`server {  `  

`    listen 443 ssl http2;`  

`    ....`  

`    ssl_certificate /etc/letsencrypt/live/example.com/fullchain.pem;`  

`   ssl_certificate_key /etc/letsencrypt/live/example.com/privkey.pem;`  

`    ssl_dhparam /etc/nginx/ssl/dhparam.pem;`  

`    ssl_trusted_certificate /etc/letsencrypt/live/example.com/root_ca_cert_plus_intermediates;`  

`    resolver ;`  

`    ....`  

`} `  

ssl_sertificate 对应的是fullchain.pem  而 ssl_certificate_key对应 privkey.pem  
ssl_dhparam用linux自带的openssl生成即可

`$ sudo mkdir /etc/nginx/ssl`  
`$ sudo openssl dhparam -out /etc/nginx/ssl/dhparam.pem 2048`

而ssl_trusted_certificate需要从letsencrypt上下载并且和自己chain.pem拼成

`$ cd /etc/letsencrypt/live/example.com`  
`$ sudo wget https://letsencrypt.org/certs/isrgrootx1.pem`  
`$ sudo mv isrgrootx1.pem root.pem`  
`$ sudo cat root.pem chain.pem > root_ca_cert_plus_intermediates`  

而resolver只需填写服务器提供商的dns即可，比如阿里的223.6.6.6 223.7.7.7
ok，Nginx配置成功，重启nginx服务

`$ sudo service nginx restart`

**3.让certbot自动更新证书**
方法很多，这里使用的是cron的daily脚本来实现自动更新

在/etc/cron.daily目录下创建一个脚本，比如autocertbot.sh
内容如下
`#!/bin/sh `
`certbot renew`
`service nginx restart`

然后修改crontab的配置文件
`$ sudo crontab -e`

加入一行 

`40 4 * * * /etc/cron.daily/autocertbot.sh 2>&1 > /dev/null`

即每天4点40分运行此脚本
ok大功告成


#### 一些有的没的

1.在线工具测试服务器SSL安全性，一般A+证明没有问题  地址：
[SSL Server Test](https://www.ssllabs.com/ssltest/index.html)  

2.我的nginx配置文件，以便日后查看 地址：
[shuyangzhang@Github](https://github.com/shuyangzhang/nginx)  

3.本文的主要参考来源于 
[letsencrypt-ssl-https](https://ksmx.me/letsencrypt-ssl-https/)
感谢原作者的无私奉献，祝开源社区越来越好  

4.最后祝你身体健康，再见！

