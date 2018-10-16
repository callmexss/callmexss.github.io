---
title: Linux生存指南
date: 2018-10-13 20:43:41
categorises:
tags:
  - linux
  - git
---

使用linux过程中最基本、最常用的命令、工具记录。

## 常用命令

### 基础命令

- ls - List directory contenets

```sh
# 显示所有文件，包括隐藏文件
ls -a

# 以详细信息显示所有文件，搭配 -a 显示所有文件的详细信息
ls -l

# 以人类可读的文件大小显示文件
ls -lh
```

- cd - Change current working directory

```sh
# 切换到指定文件夹
cd /path/to/directory

# 切换到当前用户的家目录
cd

# 切换到上一级目录
cd ..

# 切换到之前的目录
cd -
```

- mkdir - Create a directory

```sh
# 创建一个文件夹
mkdir directory

# 递归的创建目录
mkdir -p /path/to/directory
```

- rm - Remove files or directoreis

```sh
# 注意：使用rm删除的文件（夹）无法还原

# 删除文件
rm filename

# 递归删除文件夹
rm -r folder

# 强制删除一个文件夹(慎用)
rm -rf folder
```

- mv - Move or rename files and directories

```sh
# -v for verbose, -i for interactive

# 重命名文件（夹）
mv name1 name2

# 移动文件（夹）
mv /path/from/name /path/to/name
```

- cp - Copy files or folders

```sh
# -r for recursively, -v for verbose, -i for interactive, name can be changed

# 复制文件（夹）
cp /path/from/name /path/to/name
```

- cat - Print and concatenate files

```sh
# 显示文件内容
cat filename
```

- adduser - User addition utility

```sh
# 创建一个新用户，并创建默认的用户家目录，提示用户设置密码
adduser username

# useradd username 只创建一个用户，别的啥都不干
```

- usermod - Modifies a user account.

```sh
# 将一个用户添加到sudo组
usermod -aG sudo user # 其中a:表示添加，G：指定组名

# 将用户从sudo组移除
usermod -G group user
```

- chown - Change user and group ownership of files and folders.

```sh
# 改变 file/folder 的所有者:
    chown user path/to/file

# 改变 file/folder 的所有者和组:
    chown user:group path/to/file

# 递归的改变所有者:
    chown -R user path/to/folder
```

- chmod - Change the access permissions of a file or directory.

```sh
# 给定拥有文件的 [u]ser 执行文件的权限:
    chmod u+x file

# 赋予文件执行的权限
    chmod +x file
```

### 高阶命令

- find - Find files or directories under the given directory tree, recursively.

```sh
# 根据名称查找文件，支持正则表达式
find path -name given_name
```

- grep -  Matches patterns in input text

```sh
# 显示当前目录下的所有包含py的文件
ls -al | grep py
```

- ps - Information about running processes.

```sh
# Search for a process that matches a string
ps aux | grep string
```

- kill - Sends a signal to a process, usually related to stopping the process.

```sh
# 杀死一个进程
kill -9 PID
```

- 管道: |;  重定向: > >>; 通配符: * ? [] . ^ $

```sh
# 管道将上一个命令的输出作为下一条目录的输入

# 获取passwd文件里和root有关的内容
cat /etc/passwd | grep root

# 重定向: > 覆盖; >> 追加
$ echo "hello" > test
$ cat test
hello

$ echo "world" > test
$ cat test
world

$ echo "hello" >> test
$ cat test
world
hello

# 通配符
# *  任意字符任意次
# ?  任意字符一次
# [] 可匹配字符在的任意一个
```

## 常用操作

### 创建用户并添加到sudo用户组

#### 创建用户及其目录

创建一个用户jc 这个用户只能在/home/jc上面增加删除文件， jc不能在其他目录加减文件

```sh
useradd -d /home/jc -m jc
passwd jc
chown jc -R /home/jc

# adduser 也可以
```

#### 将用户添加到sudo目录

通过命令： id username
来查看用户信息

安装ubuntu时，创建了一个普通用户，没有sudo权限，执行sudo相关命令失败，原因该普通用户没有加到超级用户组，

使用如下命令可以添加到用户组（也可是超级用户组）。

命令如下：

```sh
sudo usermod -aG sudo username

# 其中a:表示添加，G：指定组名
```

### 远程登录和复制文件

#### 远程登录

- 远程登录即通过 SSH 客户端链接运行了 SSH 服务器的远程机器上。
- SSH 是目前较可靠，专为远程登录会话和其他网络服务提供安全性的协议。
  - 有效防止远程管理过程中的信息泄露。
  - 对所有传输的数据进行加密，也能防止 DNS 欺骗和 IP 欺骗。
- SSH 客户端是一种使用 Secure Shell 协议连接到远程计算机的软件程序。
- SSH 客户端简单使用访问服务器：ssh [-p port] user@remote
  - user 是远程机器上的用户名。
  - remote 是远程机器地址，可为 IP、域名或别名。
  - port 是 SSH 服务器监听的端口，若不指定端口默认为 22。

#### 远程复制文件

SCP 即 Secure Copy，是一个在 Linux 下用来进行 远程拷贝文件 的命令。

##### 从本地复制文件到远程机器桌面上

```sh
scp -P sample.py user@remote:Desktop/sample.py
```

##### 从远程机器桌面上复制文件夹到本地上

```sh
scp -P port -r user@remote:Desktop/sample ~/Desktop/sample
```

### ssh 高级用法

#### 免密码登录

免密码登录：即客户端访问服务端时，需要密码验证身份登录。

- Step.01. 配置公钥：执行 ssh-keygen 即生成 SSH 密钥。
- Step.02. 上次共钥到服务器：执行 ssh-copy-id -p port user@remote，让远程服务器记住我们的 公钥。

> 1) 有关 SSH 配置信息都保存在 /Home/yousr username/.ssh 目录下。
> 2) 免密登录使用的是非对称加密算法 ( RSA )，即使用公钥加密的数据，需要使用私钥解密；使用私钥加密的数据，需要使用公钥解密。

#### 配置别名

配置别名：每次输入 ssh -p port user@remote 是非常繁琐重复的工作，配置别名的方式以替代上述这么一串命令代码。

- 在 /.ssh/config 文件下追加以下内容 ( 需建立 Config 文件 )：

```sh
Host mac
HostName 192.168.10.1
User user
Port 22
```

命令输入 ssh mac 即可实现远程登录操作 ( SCP 同样原理 )。

```sh
scp -P 22 -r ~/Desktop/Sample mac:Desktop/Sample
```

## shell 相关操作

### bash

### fish

### zsh

### xonsh

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

## 参考

[1. ubuntu下给用户添加sudo权限，并且如何取消sudo权限](https://blog.csdn.net/u011774239/article/details/48463393)  

[2. linux下创建用户并且限定用户主目录](http://blog.sina.com.cn/s/blog_47051c800100oegn.html)