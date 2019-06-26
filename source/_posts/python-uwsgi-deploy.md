---
title: django-uwsgi-deploy
date: 2018-12-04 15:52:45
tags: 
- uwsgi
- django
categories:
- 技术类
- python

description: django-uwsgi部署到服务器
---

#### 连接服务器
```
ssh xxx@xx.xxx.x.xxx
```
#### 下载python到服务器
```
 wget https://www.python.org/ftp/python/3.7.1/Python-3.7.1.tgz  ##python官网上下载python
 
 tar -xzvf Python-3.7.1.tgz -C  /tmp  ##解压下载的文件到临时目录
 
 cd  /tmp/Python-3.7.1/  ## 进入临时目录
```
#### 把python安装到/usr/local目录
```
 ./configure --prefix=/usr/local  ##用来生成makefile，为下一步编译作准备，安装到/usr/local/bin目录下

 make   ## 用来编译

 make altinstall ## 用来安装
```
#### 如果make的时候出错自行百度一下，python3.7和3.6有点差别，emmm

#### 更改/usr/bin/python链接
```
ln -s /usr/local/bin/python3.7 /usr/bin/python3
```

#### 至此我们的准备python已经安装好，可以输入 python3查看

#### 安装mysql 
    yum install python-devel mariadb-devel -y
    pip install mysqlclient
    sudo yum install mariadb-server
> 因为centos 默认安装了mariadb,所以我们这里就用mariadb，mariadb是mysql的一个分枝，基本上可以实现mysql所有功能
    
    启动， 重启
    sudo systemctl start mariadb
    sudo systemctl restart mariadb
> 设置bind-ip

    vim /etc/my.cnf
    在 [mysqld]:
        下面加一行
        bind-address = 0.0.0.0
> 设置外部ip可以访问

    先进入mysql才能运行下面命令:
            mysql 直接进入就行
        GRANT ALL PRIVILEGES ON *.* TO 'root'@'%' IDENTIFIED BY 'pass' WITH GRANT OPTION;    
        FLUSH PRIVILEGES
> 设置阿里云的对外端口

        
#### 安装nginx
```
https://www.digitalocean.com/community/tutorials/how-to-install-nginx-on-centos-7
```
#### 安装virtualenv
```
yum install python-setuptools python-devel
pip install virtualenvwrapper
```
#### 编辑.hashrc 
```
vim ~/.bashrc  ##当前文件下面加入下面两行
export WORKON_HOME=$HOME/.virtualenvs
source /usr/bin/virtualenvwrapper.sh
```
#### 重新加载.bashrc文件
```
source  ~/.bashrc
```
> 1. 新建虚拟环境

    mkvirtualenv xxx -p(可指定python版本号) root(python路径) 

> 2. 进入虚拟环境 
workon xxx

> 3. 然后安装pip包
    通过 `pip freeze > package.txt` 将本地的虚拟环境安装包相信信息导出来
    将package.txt文件上传到服务器;

> 4. 安装依赖包    
    `pip install -r package.txt`
    

#### 安装uwsgi
```
pip install uwsgi
```

#### 在你的Django项目中新建一个uwsgi.ini
```
使用vim新建这个文件 添加如下代码
[uwsgi]
socket = 0.0.0.0:8000   ##这个地方是你后端服务的端口号(用nginx的时候就配socket，直接运行的时候配 http)
chdir=/root/test  ##这个是你的工程项目的绝对地址
module=test.wsgi  ##这个就是你项目自己创建的文件夹里的wsgi文件
master = true     ##允许主线程存在    
processes=2       ##开启的进程数量
threads=2         ##进程中线程启动数量
max-requests=2000 ##为每个工作进程设置请求数的上限。当一个工作进程处理的请求数达到这个值，那么该工作进程就会被回收重用（重启）。你可以使用这个选项来默默地对抗内存泄漏
chmod-socket=664 ##读取socket权限
vacuum=true      ##当服务器退出的时候自动清理环境，删除unix socket文件和pid文件
daemonize = /wwwroot/destiny/uwsgi.log ## 使进程在后台运行，并将日志打到指定的日志文件或者服务器
virtualenv = /root/xxx/.virtualenvs/xxx ##虚拟环境绝对地址
```
#### 保存并退出文件

#### 配置nginx文件 在/etc/nginx/conf.d目录下新建web.conf 文件
```
vim /etc/nginx/conf.d/web.conf

upstream web{
    server 0.0.0.0:8000  ##要代理的项目接口
}
server {
    listen 80 ##监听的端口
    server_name crystal.ajletme.com ##你要输入在地址栏上的ip或者地址
    charst utf-8; ##这个就不用说了吧emmm
    index index.html index.htm index.php; ##首页的文件类型
    root /root/test; ##首页所在的绝对地址(下面有index.html等)
    
    location /media{
        alias /root/test/medi; ##链接到项目目录下的media(媒体类型文件)
    }
    location /static{
        alias /root/test/static; ##连接到项目目录下的static文件(静态文件)
    }
    location /api{
        uwsgi_pass web; ##这里是说uwsgi要把crystal.ajletme.com/api代理到啥地方
        include uwsgi_params; ##你安装的uwsgi参数,这个文件和nginx.conf在同一级
    }
    
}
```
#### 退出保存
 
```
重启nginx
sudo nginx -s reload
启动 uwsgi
sudo uwsgi -ini /root/test/uwsgi.ini
```

#### 在浏览器里打上你要nginx配置的server_name就能看到你想看到页面了 不出意外的话，如果报错 可以再找找原因emmmm

#### 如果你成功打开api接口，你会发现没有rest_framework的样式都没有了，浏览器控制台会出现各种静态文件路径404，别着急，是因为你没有把drf的静态文件抽离出来，具体做法如下
#### 还记得我们再nginx里面配置的static代理目录么，它就是用来干这个事的
```
在你的项目目录中运行  python manage.py collectstatic 
```
#### Django项目中本来是没有static目录的，运行这条命令之后会生成一个static目录，存放的是我们项目中用到的js,css等静态文件。我们把这个文件传到服务器上，这个时候再打开网页就能看到样式回来了。如果没有生效，再重启一下nginx。毕竟重启大法好！！