---
layout: post
title: 树莓派3+ Tornado 实现大文件上传系列（一）—— 前期准备
tags: [Code]
---
宿舍里有闲置的树莓派3一块，移动硬盘一块，于是突发奇想，想做一个网盘。刚开始的时候以为很简单，直到以来一坑接着一坑不断向我袭来... 幸好最后都被一一解决了。其中各种心酸各种查资料各种摸索...

好吧进入正题！

## 硬件方面的配置
- 树莓派3（系统 Raspbian）
- Toshiba 的移动硬盘 1T（ntfs格式）
- 有源HUB - ORICO 7926-U3（树莓派接移动硬盘必须要用有源HUB，否则电压不够）

## 软件方面的安装与配置
- web服务器：tornado
- web反向代理:nginx（附带文件上传模块 nginx upload module）
- 数据库：mysql

### 用 nginx 作反向代理的原因
tornado 在处理文件上传时，会将文件储存在内存中，因此并不适合直接用于大文件上传，只好用 nginx 作代理，借助其 upload module 先将文件暂时存到某个地方而后再让 tornado 处理之。

### 挂载移动硬盘
插入移动硬盘后，查看系统的硬盘和硬盘分区情况

	#sudo fdisk -l

然后通过观察 size 大小确定自己移动硬盘的 Device 是什么。我的是 /dev/sda1 ，因此以下以 /dev/sda1 为例。

下载 ntfs-3g

> NTFS-3G 是一个开源的软件，可以实现 Linux、Free BSD、Mac OSX、NetBSD 和 Haiku 等操作系统中的 NTFS 读写支持。它可以安全且快速地读写 Windows 系统的 NTFS 分区，而不用担心数据丢失。

	#sudo apt-get install ntfs-3g

建立目录用来作挂接点(mount point)

	#mkdir -p /mnt/sda1
	#mount -t ntfs-3g /dev/sda1 /mnt/sda1

挂载成功。现在可以在挂载点上对移动硬盘进行读写操作了。

### Nginx安装及配置（附带nginx upload module）

#### Nginx安装

nginx 依赖以下模块：

- gzip 模块需要 zlib 库
- rewrite 模块需要 pcre 库
- ssl 功能需要 openssl 库

安装命令
	sudo apt-get install openssl libssl-dev  
	sudo apt-get install libpcre3 libpcre3-dev  
	sudo apt-get install zlib1g-dev  

因此，在安装 nginx 之前，需先下载以上三个库的源代码（博主正是忽略了这一点，导致连续编译了好几次... 多次麻烦折腾）

而要编译安装 nginx upload module ，则需先下载其源码，**建议下载 [github上2.2分支的源码][1] ，在官网上下的源码编译会出错，以及下载 github 主分支的也会出错**。

回到 nginx 源码目录，然后 ./configure 编译 nginx ，附带参数 

	--with-openssl=<openssl_dir> --with-pcre=<pcre_dir> --with-zlib=<zlib_dir> --add-module=<uplpad_module_dir>

指定依赖的模块目录。

然后执行 make && make install 完成安装

#### Nginx  upload module 配置

在nginx配置文件的server节点增加如下配置，假设http://hostname/upload地址用户文件上传。

	location /upload {
	    upload_pass /api/upload;
	    upload_cleanup 400 404 499 500-505;
	    upload_store /mnt/sda1/upload_tmp;
	    upload_store_access user:r;
	    upload_limit_rate 0;
	    upload_set_form_field "file_name" $upload_file_name;
	    upload_set_form_field "content_type" $upload_content_type;
	    upload_set_form_field "tmp_path" $upload_tmp_path;
	    upload_aggregate_form_field "md5" $upload_file_md5;
	    upload_aggregate_form_field "size" $upload_file_size;
	    upload_pass_form_field "^.*$";
	}

各参数说明如下：

> upload\_pass 指明了需要后续处理的php/tornado地址
> 
> upload\_cleanup 如果php出现400 404 499 500-505之类的错误，则删除上传的文件
> 
> upload\_store 上传文件存放地址
> 
> upload\_store\_access 上传文件的访问权限，user:r是指用户可读\_
> 
> upload\_limit\_rate 上传限速，如果设置为0则表示不限制\_
> 
> upload\_set\_form\_field 设定额外的表单字段。这里有几个可用的变量：
> 
> - $upload\_file\_name 文件原始名字
> - $upload\_field\_name 表单的name值
> - $upload\_content\_type 文件的类型
> - $upload\_tmp\_path 文件上传后的地址
> - upload\_aggregate\_form\_field 额外的变量，在上传成功后生成
> - $upload\_file\_md5 文件的MD5校验值
> - $upload\_file\_size 文件大小
> - upload\_pass\_form\_field 从表单原样转到后端的参数，可以正则表达式表示。官方的例子是upload\_pass\_form\_field \"\^submit\$\|^description\$\";意思是把submit，description这两个字段也原样通过upload\_pass传递到后端php处理。如果希望把所有的表单字段都传给后端可以用upload\_pass\_form\_field \"^.\*\$\";

我们上传文件存储到临时目录后nginx立即会向后面的tornado服务的/api/upload发起一个表单请求,其中有五个表单项：

- file\_name 上传的文件名
- content\_type 文件的MIME类型
- tmp\_path 临时文件目录
- md5 文件的md5值
- size 文件大小

这时就已经可以在 tornado 中对文件进行处理了。

另：启动、停止、重启 Nginx 命令如下 ：

	sudo /usr/local/nginx/sbin/nginx
	sudo /usr/local/nginx/sbin/nginx -s stop
	sudo /usr/local/nginx/sbin/nginx -s restart

以上两个是我在配置过程中比较坑的点（特别是 nginx 的 upload module ，由于作者五年前就停更了，因此官网上的源码似乎不能兼容新版 nginx，这个真的是非常非常的坑......），至于其他的安装 Mysql 、tornado 等等的在此就忽略不谈了 :)

[1]:	https://github.com/vkholodkov/nginx-upload-module/tree/2.2