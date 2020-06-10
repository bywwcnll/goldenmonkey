---
title: dnsmasq教程
date: 2020-06-10 16:28:45
tags:
- dns
- dnsmasq
- caddy
---

## 安装

``` bash
brew install dnsmasq
```

## 配置

将`/usr/local/etc/dnsmasq.conf`中的`conf-dir`设置为`/usr/local/etc/dnsmasq.d`,再在`/usr/local/etc/dnsmasq.d`路径下创建`address.conf`

``` js
strict-order
listen-address=127.0.0.1
listen-address=0.0.0.0
address=/.try/192.168.137.241
server=114.114.114.114
```

## 启动

``` bash
sudo brew services start dnsmasq
```
