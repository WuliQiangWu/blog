---
title: 部署 Hexo 到服务器
date: 2018-11-27 17:20:09
tags: 
- Hexo 
- linux
- 服务器
categories:
- 技术类
- 服务器部署
description: 一步一步把hexo项目部署到自己服务器
---

#### 首先要在服务器建一个存放hexo项目的仓库，这里使用git
##### 查看是否存在git. 如无，则安装
     git --version 
    
     yum install curl-devel expat-devel gettext-devel openssl-devel zlib-devel perl-devel
     yum install -y git
##### 新建一个git用户并配置仓库
    useradd git
    passwd git                  // 设置密码
    su git                      // 这步很重要，不切换用户后面会很麻烦
    cd /home/git/
    mkdir -p projects/blog      // 项目存在的真实目录
    mkdir repos && cd repos
    git init --bare blog.git    // 创建一个裸露的仓库
    cd blog.git/hooks
    
    vim post-receive             // 创建hook钩子函数，输入了内容如下
 
    #!/bin/sh
    git --work-tree=/home/git/projects/blog --git-dir=/home/git/repos/blog.git checkout -f

##### 上面步骤走完之后，要给git用户修改权限
      chmod +x post-receive        // 添加执行权限
      exit                         // 退出到 root 登录
      chown -R git:git /home/git/repos/blog.git     // 改变 blog.git 目录的拥有者为 git 用户
      
##### 测试你的仓库是否可用
        git clone git@yourserver_ip:/home/git/repos/blog.git
        如果拉下来一个空仓库，就表示仓库建成功

##### 避免每次提交代码都要输入密码，要添加本地信任
        ssh-copy-id -i /你的用户/.ssh/id_rsa.pub git@yourserver_ip    //绝对路径
        ssh git@yourserver_ip  // 此时登陆不需要密码，如果需要就表示之前配置出错，再检查一遍
    
#### 使用nginx 部署 打开
###### 进入nginx文件夹 
        cd /etc/nginx/
        先进入 nginx.conf 文件修改 user 为 root
        然后保存退出
        进入conf.d 文件夹，这个是nginx的默认启动文件
        添加并编辑 blog.conf
        vim blog.conf
>     
  
        server {
                listen      80;  # 默认启动端口
                server_name yourserver_ip; # 你要再浏览器里打开的网址或者IP地址
                charset     utf-8; 
                index index.html index.htm index.php;
                client_max_body_size 75M;   # adjust to taste
        
                location  / {
                        root    /home/git/projects/blog; # 项目保存的地址
                        index index.html index.htm index.php;
                }
        }
>      

        编辑完成之后保存退出
        
        nginx -s reload  //重启nginx
###### 当你打开网站的时候可能会抱403错误，因为nginx没有权限访问 /home 目录下的文件，需要给/home 添加访问权限
         chmod -R 777 /home/git 
    
        
#### hexo 项目中的_config.yml 文件中 添加
        deploy:
          type: git
          repo: git@yourserver_ip:/home/git/repos/blog.git
          branch: master

#### hexo 项目中的package.json 文件添加
        "scripts": {
          "deploy": "hexo clean && hexo g -d",
          "start": "hexo clean && hexo g && hexo s"
        },
        
        最后直接输入 npm run delpoy 直接部署到服务器