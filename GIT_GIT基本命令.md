---
title: GIT基本命令
date: 2021-11-29 13:36:37
tags:
categories: GIT
cover:
---
# 理论基础

git工作流程图：

![git工作流程图](https://saladday-figure-bed.oss-cn-chengdu.aliyuncs.com/img/20200811162120381.png)

## Git的数据库分为远程数据库和本地数据库的两种。

- 远程数据库: 配有专用的服务器，为了多人共享而建立的数据库。
- 本地数据库: 为了方便用户个人使用，在自己的机器上配置的数据库。

数据库分为远程和本地两种。平时用手头上的机器在本地数据库上操作就可以了。如果想要公开在本地数据库中修改的内容，把内容上传到远程数据库就可以了。另外，通过远程数据库还可以取得其他人修改的内容。

![远程数据库和本地数据库](https://saladday-figure-bed.oss-cn-chengdu.aliyuncs.com/img/capture_intro1_2_2.png)

## 工作树和索引

在git的管理下，用户的工作区一般被叫做工作树；

在数据库和工作树之间有索引，索引是向数据库提交作准备的区域；如图：

![工作树和索引](https://saladday-figure-bed.oss-cn-chengdu.aliyuncs.com/img/capture_intro1_4_1.png)

Git在执行提交的时候，并不是直接将工作树的直接保存到数据库，而是将设置在中间索引区域的文件保存到数据库。

因此，需要提交文件，首先需要将文件加入到索引区中。





## 修改记录的提交

若要把文件或目录的添加和变更保存到数据库，就需要进行提交。

执行提交后，数据库中会生成上次提交的状态与当前状态的差异记录（也被称为revision）。

注意：

1.  不同类别的修改 (如：Bug修复和功能添加) 要尽量分开提交，以方便以后从历史记录里查找特定的修改内容；

2.  注意写提交信息，这是你自己review代码或者别人理解你代码的重要信息；



## 合并修改记录

在执行Pull之后，进行下一次push之前，如果其他人进行了推送内容到远程数据库的话，那么你的push将被拒绝。

需要解决冲突：

如果远程数据库和本地数据库的同一个地方都发生了修改的情况下，因为无法自动判断要选用哪一个修改，所以就会发生冲突。

![image-20221130132849129](https://saladday-figure-bed.oss-cn-chengdu.aliyuncs.com/img/image-20221130132849129.png)

git会把当前分支的修改和 合并操作的修改记录在文件中，需要程序员去修改。

处理完冲突之后，gitadd &&commit 将解决冲突后的文件提交;

## 开发中分支使用流程和原则

- master分支------线上分支
- develop分支------是从master创建的分支,一般作为开发部门的主要开发分支;更新到一定版本合并到master分支
- feature分支------从develop分支分出,保证不同开发之间不相互影响,开发完成合并到develop分支就删除
- hotfix分支------发现线上分支出现bug,开出分支修改,修改完成同时merge到develop和master分支





## 克隆远程数据库

即从云端复制一个仓库

不会很平凡,一般只在第一次的时候clone

clone操作会完整的将远程数据库复制到本地数据库。

clone后的远端数据库的变更log也会被复制，所以可以像操作原始数据库一样进行查看记录和其他操作。

## 删除本地分支

```bash
$git branch -d dev01
```



## 删除远程分支

```bash
$ git push origin --delete master
```

## 添加本地分支

```bash
1.移动并且创建分支
git checkout -b dev01
2.直接创建
git branch dev01
```



## 添加远程分支

```bash
$ git checkout -b dev01 //相当于git branch dev01 && git checkout dev01
$ git push origin dev01 //把这个分支push上去
```

## 连接本地分支和远程分支

```bash
1.查看本地分支和远程分支的关联关系
git branch -vv
2.创建连接关系
$ git branch --set-upstream-to=origin/main main


```



----



# 命令行使用GIT

### 1.  在workplace打开gitbash，利用```git init```命令将该目录移动到本地Git数据库(不可见)，默认名字为master

### 2. 可以使用status命令确认工作树的状态

<img src="https://saladday-figure-bed.oss-cn-chengdu.aliyuncs.com/img/image-20221129135551592.png" alt="image-20221129135551592" style="zoom: 50%;" />

说明此时master分支还没有东西，并且索引也没有任何东西，所有的文件还只是在工作树中。

### 3. 将文件加入到索引，要使用add命令。

```bash
$ git add <file>.. //在<file>指定加入索引的文件。用空格分割可以指定多个文件。
$ git add . //指定参数. 可以把所有的文件加入到索引

```

### 3. 此时所有文件已经被加入到索引区了，我们要将其提交到本地服务器中。

```bash
$ git commit -m "first(init) commit"

$ git status //查看此时索引和工作树的状态
On branch master
nothing to commit, working tree clean

$ git log //可以查看提交记录  （git lg为优化的log命令）
Author: saladday <1203511142@qq.com>
Date:   Tue Nov 29 14:03:10 2022 +0800

    init commit


```



### 4. 提交到本地服务器之后，我们需要提交至远程服务器。首先需要连接远程仓库，我们采用SSH免密登入的方式。

```bash
1.首先在本地创建一对密钥。
$ ssh-keygen -t rsa -b 4096 -C "1203511142@qq.com"
2.将公钥保存至GitHub服务器
（测试用）3.利用ssh免登录连接到github服务器
$ ssh -T git@github.com
Hi SaladDay! You've successfully authenticated, but GitHub does not provide shell access.


4.Github仓库和本地仓库连接,代码中的origin是远程代码仓库的别称，也可以改成别的。
$ git remote add origin <repository url>
```

### 5.  此时已经将本地仓库连接上远程仓库了，我们可以向上push代码了。

```bash
$ git push -u origin master //-u的作用：下一次push的时候就不用指定远程分支了

//一般我们修改部分代码的话，可以先用git status查看一下修改的东西
//然后git add 将其添加到索引
//然后git commit 将其提交至本地服务器
//然后git push 将其提交至远程服务器仓库


```

