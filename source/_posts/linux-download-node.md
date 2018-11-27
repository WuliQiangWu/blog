---
title: Linux安装nodejs并升级版本
date: 2018-11-27 18:28:37
tags:
- node 
- linux
categories:
- 技术类
- Linux
description: Linux系统下安装nodejs并升级版本
---
#### 下载安装nodejs
##### node有不同平台的版本，所以选择对应的Linux版本的node，这里还要先知道自己Linux系统是32位还是64位的
     通过  uname -a  命令查看到我的Linux系统位数是64位（ps：x86_64表示64位系统， i686 i386表示32位系统)根据自己的系统下载对应的Linux版本
    
![linux-version](/images/linux-version.jpg)
    
     下载node到本地
     wegt https://nodejs.org/download/release/v11.2.0/node-v11.2.0-linux-x64.tar.xz

#### 解压包
     1. tar -xvf node-v11.2.0-linux-x64.tar.xz
     2. 查看解压之后的文件目录下bin目录里是否有node 和 npm
     3. 如果没有要添加软连接到/usr/local/bin目录下
        ln -s  /xx/node-v11.2.0-linux-x64.tar.xz/bin/node   /usr/local/bin
        ln -s  /xx/node-v11.2.0-linux-x64.tar.xz/bin/npm   /usr/local/bin
        (ps 这个路径必须是绝对路径)
    
> 输入node -v 查看当前node版本
> 输入npm -v 查看当前npm版本 (npm是node的包管理器，安装node成功之后自己就会安装一个版本)

#### 升级nodejs版本
    清除node的缓存
    npm cache clean -f
    
    安装 node的 n 模块 
    npm install -g n
    
    安装node最新版本
    n stable  
    
    安装完成之后查看node版本
    node -v