---
title: Git 简单操作
date: 2018-11-23 17:06:17
tags: git
categories:
- 技术类
- git
description: Git 常用操作命令
---

#### 本地代码与远程仓库之间的链接

```
git init
git add  .
git  commit  -m 'init'
git remote add origin  URL //加入到远程的Git仓库
git push -u origin master  //将项目推到Git仓库

```
#### 此后每次更新代码 如果仓库里面的代码有更改，上传之前需要先执行一下pull操作
```
git pull  将仓库已有代码拉到本地做一个更新
git add .   "."的意思是所有更新的文件
git commit -m 'xxx'  做一次提交 'xxx'是对本次提交的注释
git push 将代码推到远程仓库

```
#### 更换远程仓库地址
```
git remote -v    //查看远程仓库地址
git remote set-url origin URL    //更换远程仓库地址
git remote -v    //查看远程仓库地址

```
#### pull 代码的时候 要在本地删除之前远程仓库删除掉的分枝
```
git pull --prune 
```
#### 本地仓库名称重命名
```
git branch -m new-name
```

#### 删除本地分枝
```
git branch -D branch-name

```
#### 回滚分枝，需要log-id 不知道id 的可以 使用 git log 命令查看提交日志,log-id 指的是 git log 打出来的 commit  后面 指向的一串数字字母组成的40位序列
```
git log

git reset --hard  log-id
```

#### 强制覆盖远程分枝(如:master)的代码
```
git push origin master -f
```