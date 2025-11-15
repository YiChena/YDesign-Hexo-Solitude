---
title: Hexo
date: 2020-12-09 16:17:28
cover: https://ydesign.oss-cn-beijing.aliyuncs.com/Hexo.png
# type: "技术"
categories:
    - 技术教程
tags:  
    - 技术
    - Hexo
---

# Hexo
```
hexo clean && hexo g && hexo s 
```

```
hexo clean && hexo g && hexo d
```

```
git init ：初始化
git add .
git commit -m "更新"
git push origin main
```

```
git push -f ：强制推送
git push -f origin main ：将本地分支推送到远程仓库并设置上游分支
```

```
git fetch origin ：更新本地仓库的远程跟踪分支信息，从远程仓库（origin）获取最新的分支和提交信息，但不会改变你当前的工作目录或分支
```

```
git branch ：查看当前所在的分支
git branch -r ：查看远程仓库中所有的分支
git branch -a ：查看所有分支


git checkout main：当前分支不是 main，你需要切换到 main 分支

git checkout -b main： 创建并切换到 main 分支

git checkout main

git pull origin main --rebase
```

```
git remote add origin https://github.com/... ：添加仓库

fatal: unable to access 'https://github.com/...': SSL peer certificate or SSH remote key was not OK

解决方案：git config --global http.sslVerify "false"
```



