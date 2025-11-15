---
title: Git
date: 2020-11-12 00:00:00
cover: https://ydesign.oss-cn-beijing.aliyuncs.com/Git.png
tags: 
    - 技术
    - 工具
    - Git
---

# Git

 clear 清空

### 创建文件

 touch file02.txt

### 查看修改的状态（status）

- 作用：查看的修改的状态（暂存区、工作区）

- 命令形式：git status

### 添加工作区到暂存区(add)

- 作用：添加工作区一个或多个文件的修改到暂存区 

- 命令形式：git add 单个文件名|通配符 
  - 将所有修改加入暂存区：git add .

### 提交暂存区到本地仓库(commit)

- 作用：提交暂存区内容到本地仓库的当前分支 
- 命令形式：git commit -m '注释内容

### 查看提交日志(log)

- 作用:查看提交记录 
- 命令形式：git log [option] 
  - options 
    - --all 显示所有分支 
    - --pretty=oneline 将提交信息显示为一行 
    - --abbrev-commit 使得输出的commitId更简短 
    - --graph 以图的形式显示



$ git log --pretty=oneline --abbrev-commit

$ git log --pretty=oneline --abbrev-commit --all --graph

###  版本回退

- 作用：版本切换 
- 命令形式：git reset --hard commitID 
  - commitID 可以使用 git-log 或 git log 指令查看 

- 如何查看已经删除的记录？

  -  git reflog 

  - 这个指令可以看到已经删除的提交记录



### 添加文件至忽略列表 

一般我们总会有些文件无需纳入Git 的管理，也不希望它们总出现在未跟踪文件列表。 通常都是些自动 生成的文件，比如日志文件，或者编译过程中创建的临时文件等。 在这种情况下，我们可以在工作目录 中创建一个名为 .gitignore 的文件（文件名称固定）



### 分支

几乎所有的版本控制系统都以某种形式支持分支。 使用分支意味着你可以把你的工作从开发主线上分离 开来进行重大的Bug修改、开发新的功能，以免影响开发主线。

#### 查看本地分支 

- 命令：git branch

#### 创建本地分支 

- 命令：git branch 分支名 

#### *切换分支(checkout) 

- 命令：git checkout 分支名 

我们还可以直接切换到一个不存在的分支（创建并切换） 

- 命令：git checkout -b 分支名

#### *合并分支(merge) 

一个分支上的提交可以合并到另一个分支 

命令：git merge 分支名称

#### 删除分支 

不能删除当前分支，只能删除其他分支 

git branch -d b1 删除分支时，需要做各种检查 

git branch -D b1 不做任何检查，强制删除

#### 开发中分支使用原则与流程 

几乎所有的版本控制系统都以某种形式支持分支。 使用分支意味着你可以把你的工作从开发主线上分离 开来进行重大的Bug修改、开发新的功能，以免影响开发主线。

 在开发中，一般有如下分支使用原则与流程： 

- master （生产） 分支 

线上分支，主分支，中小规模项目作为线上运行的应用对应的分支； 

- develop（开发）分支 

是从master创建的分支，一般作为开发部门的主要开发分支，如果没有其他并行开发不同期上线 要求，都可以在此版本进行开发，阶段开发完成后，需要是合并到master分支,准备上线。 

- feature/xxxx分支 

从develop创建的分支，一般是同期并行开发，但不同期上线时创建的分支，分支上的研发任务完 成后合并到develop分支。 

- hotfix/xxxx分支， 

从master派生的分支，一般作为线上bug修复使用，修复完成后需要合并到master、test、 develop分支。 

- 还有一些其他分支，在此不再详述，例如test分支（用于代码测试）、pre分支（预上线分支）等 等。

#### 分支操作

```shell
###########################创建并切换到dev01分支，在dev01分支提交
# [master]创建分支dev01
git branch dev01
# [master]切换到dev01
git checkout dev01
# [dev01]创建文件file02.txt
略
# [dev01]将修改加入暂存区并提交到仓库,提交记录内容为：add file02 on dev
git add .
git commit -m 'add file02 on dev'
# [dev01]以精简的方式显示提交记录
git-log
###########################切换到master分支，将dev01合并到master分支
# [dev01]切换到master分支
git checkout master
# [master]合并dev01到master分支
git merge dev01
# [master]以精简的方式显示提交记录
git-log
# [master]查看文件变化(目录下也出现了file02.txt)
略
##########################删除dev01分支
# [master]删除dev01分支
git branch -d dev01
# [master]以精简的方式显示提交记录
git-log
```



### Git远程仓库

 
