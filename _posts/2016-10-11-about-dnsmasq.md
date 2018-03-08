---
layout: post
title: dnsmasq在树莓派上的安装与配置
tags: [Code]
---

DNSmasq 是一个小巧且方便地用于配置 DNS 和 DHCP 的工具，适用于小型网络，它提供了 DNS 功能和可选择的 DHCP 功能。它服务那些只在本地适用的域名，这些域名是不会在全球的 DNS 服务器中出现的，下面我们来看看如何使用 DNSmasq 搭建局域网 DNS 小型服务器。

### Dnsmasq 安装
	sudo apt-get install dnsmasq

### 建立 DNSmasq 配置文件并添加内容
	sudo vi /etc/dnsmasq.conf


在配置文件中添加：

	resolv-file=/etc/resolv.dnsmasq.conf
	addn-hosts=/etc/dnsmasq.hosts
	listen-address=127.0.0.1,192.168.1.1 #此处可填写 127.0.0.1，服务器的本地 ip、公网 ip 等，效果大家都懂的。
	address=/test.com/127.0.0.1 # 域名解析

resolv.dnsmasq.conf 与 dnsmasq.hosts 的配置方式和系统文件中的 resolv.conf 以及 hosts 相同，因此方便起见，可以直接复制系统的这两个文件的内容分别到 resolv.dnsmasq.conf 与 dnsmasq.hosts 中。

### DNSmasq 设置启动和测试
	/etc/init.d/dnsmasq restart
