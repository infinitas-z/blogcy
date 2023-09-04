---
title: Git基本使用
date: 2023-07-23 14:08:06
tags: [git]
top_img: https://pic.imgdb.cn/item/64bd07b71ddac507cc6e18e1.png
cover: https://pic.imgdb.cn/item/64bd07b71ddac507cc6e18e1.png
---

## Git 基本使用

### 创建，复制项目，仓库

在目录中执行 **git init** 就可以创建一个 Git 仓库了

```
$ git inint  
```

### 项目克隆 

复制项目的url

```
$ git clone xxx(项目url)
```

### 文件状态,修改

在生成版本时，未被跟踪的文件不会在版本中

```
某个文件跟踪,变成暂存状态
$ git add <name>  
某个文件目录下所有文件进行跟踪，“.”表示当前路径
$ git add .  
变成未跟踪状态
$ git rm <name>
保留在目录里但是改成未被跟踪状态
$ git rm --cache <name>
```

 修改文件

```
文件改成暂存状态
$ git add <file-name>
取消暂存状态
$ git reset head <name>
撤销提交，不能撤销第一次的提交
$ git reset head~ --soft
提交已修改的文件，形成新版本，或者自定义标识
$ git commit
$ git commit -m 'name'
```

文件的四个状态，通过不同命令改变其状态，暂存就是在上一次提交后已经被修改但是未被提交的文件

![](https://pic.imgdb.cn/item/64bccfcc1ddac507ccdea7c9.jpg)



```
查询文件状态
$ git status
查询提交历史
$ git log
$ git log --pretty=format:"%h -%an,%ar:%s"  格式化信息
格式信息
%h 简化哈希，%an 作者名字， %ar 修改日期,%ad 修订日期，%s提交说明
图形化查看
$ git log --graph
```

### 远程仓库

github举例，链接远程仓库,试验之前记得通过git init创建git仓库

```
$ git remote add name(自定义) url(远程仓库链接)
列出当前仓库中已配置的远程仓库
$ git remote
提交到远程仓库 前提ssh建权或者token建权
$ git push origin(名字) master(分支)
```

### 分支

![](https://pic.imgdb.cn/item/64bcdf3d1ddac507cc087bf7.jpg)

通过 git branch --list查看当前分支，或者通过git log，或者通过git status查看，

```
$ git log
head ->master 表示当前所处的分支，origin/master表示远程仓库所处的分支
commit 2bfd383e2cce2e8a210d7907d3fdc72fa14db69f (HEAD -> master, origin/master)
Author: infinitas-z <a2452992242@163.com>
Date:   Sun Jul 23 15:40:22 2023 +0800

    milet

```

创建，切换分支

```
创建
$ git branch feature1
切换分支
$ git checkout feature1
新建分支并且切换到新分支
$ git checkout -b feature2 
合并分支，当前分支和feature2进行合并
git merge feature2  
查看合并状态
git log --all --graph
```

示例·

```
用以上方法创建并切换到新分支feature2
新建test_file
vi test_file
暂存，提交到
git add test_file
git commit -am 'feature2'
查看状态 
git log --pretty=oneline
git checkout master
合并分支
git merge feature2
查看合并状态
git log --all -graph
提交到github，加上-u后以后推送一样的名字和分支时，就可以直接git push
git push -u origin(自定义的名字) master
```

拉取分支

```
git fetch
```

把远程分支变为本地分支，相当于新建跟踪分支

```
git cheackout -b feature1 origin/feature1
简化写法
git cheackout feature1
```

### 贮藏

把没写完的代码暂存,在我们未暂存文件时，git无法切换分支，但是我们有切换分支的需求,我们需要存储修改

```
//去存储我们的修改
$ git stash 
切换想要的分支
$ git checkout master
//恢复当前修改
$ git stash apply
查看stash状态
$ git stash list
把文件恢复成未修改的状态，慎用，无法找回
$ git checkout -- <file_name>
把最后一次存储的stars恢复,会在恢复完成后把stash删掉
$ git stash pop
要删除指定stash的话
git stash drop stash@{index(数字)}
```

### 重置

前面用到过，head表示当前提交，head~就代表上次提交，也可以用head~2代表倒数第二次提交

加入 --soft说明撤销的只是提交，文件的暂存状态还是保留的，如果不加就不保留暂存状态

```
撤销提交，不能撤销第一次的提交
$ git reset head~ --soft
撤销提交，并且撤销你修改的内容,回到上一次提交的状态，慎用
$ git rest head~ --hard
```

rebase 变基，A程序员和B程序员分别对应两个分支，把分支切换到B时，rebaseA时，就会将A的修改搬到B上，类似于搬家，进行二次开发时，就不要去做rebase操作了

rebase -i 可以进入交互界面，对提交等进行修改

都是些基础的操作，需要详细的还得去看git官方文档和github的官方文档，over！
