---
title: Linux生存指南
date: 2018-10-13 20:43:41
categorises:
tags:
  - linux
  - git
---

# Linux 存活 All in one

使用linux过程中最基本、最常用的命令、工具记录。

## 基础命令

## 用户相关操作

## shell 相关操作

## git 相关操作

### git 新建仓库并推送到远程分支

问题描述：在github上新建了仓库，如何将本地仓库推送到远程仓库?

解决方法：

```sh
echo "# repository name" >> README.md
git init # 初始化仓库
git add README.md # add the README.md file
git commit -m "first commit"
git remote add origin git@github.com:username/repository_name.git
git push -u origin master
```

git 远程仓库相关的常用命令：

```sh
# 列出每一个指定的远程服务器简写
$ git remote
origin # git 给每个仓库初始的简写


# 显示读写仓库使用的简写和其对应的<url>
$ git remote -v
origin https://github.com/schacon/ticgit (fetch)
origin https://github.com/schacon/ticgit (push)


# 添加一个新的远程 Git 仓库，同时指定仓库<url>为一个你可以轻松引用的简写<shortname>。
# git remote add <shortname> <url>
$ git remote
origin
$ git remote add pb https://github.com/paulboone/ticgit
$ git remote -v
origin https://github.com/schacon/ticgit (fetch)
origin https://github.com/schacon/ticgit (push)
pb https://github.com/paulboone/ticgit (fetch)
pb https://github.com/paulboone/ticgit (push)


# 从远程仓库抓取和拉取
# git fetch [remote-name]
$ git fetch origin # git fetch 不会自动合并或修改当前的工作
$ git pull # 抓取数据并自动尝试合并


# 推送到远程仓库
$ git push origin master


# 查看远程仓库
$ git remote show origin


# 重命名远程仓库
$ git remote rename pb paul
$ git remote
origin
paul


# 删除远程仓库
$ git remote rm paul
$ git remote
origin
```

## 工具部分