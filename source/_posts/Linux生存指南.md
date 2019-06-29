---
title: Linux生存指南
date: 2018-10-13 20:43:41
categorises:
tags:
  - linux
  - git
  - bash
  - xonsh
  - zsh
  - fish
  - tldr
  - proxychain
  - hexo
---

使用linux过程中最基本、最常用的命令、工具记录。

# 常用命令

## 基础命令

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

## 高阶命令

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

# 常用操作

## 创建用户并添加到sudo用户组

### 创建用户及其目录

创建一个用户jc 这个用户只能在/home/jc上面增加删除文件， jc不能在其他目录加减文件

```sh
useradd -d /home/jc -m jc
passwd jc
chown jc -R /home/jc

# adduser 也可以
```

### 将用户添加到sudo目录

通过命令： id username
来查看用户信息

安装ubuntu时，创建了一个普通用户，没有sudo权限，执行sudo相关命令失败，原因该普通用户没有加到超级用户组，

使用如下命令可以添加到用户组（也可是超级用户组）。

命令如下：

```sh
sudo usermod -aG sudo username

# 其中a:表示添加，G：指定组名
```

## 远程登录和复制文件

### 远程登录

- 远程登录即通过 SSH 客户端链接运行了 SSH 服务器的远程机器上。
- SSH 是目前较可靠，专为远程登录会话和其他网络服务提供安全性的协议。
  - 有效防止远程管理过程中的信息泄露。
  - 对所有传输的数据进行加密，也能防止 DNS 欺骗和 IP 欺骗。
- SSH 客户端是一种使用 Secure Shell 协议连接到远程计算机的软件程序。
- SSH 客户端简单使用访问服务器：ssh [-p port] user@remote
  - user 是远程机器上的用户名。
  - remote 是远程机器地址，可为 IP、域名或别名。
  - port 是 SSH 服务器监听的端口，若不指定端口默认为 22。

### 远程复制文件

SCP 即 Secure Copy，是一个在 Linux 下用来进行 远程拷贝文件 的命令。

#### 从本地复制文件到远程机器桌面上

```sh
scp -P sample.py user@remote:Desktop/sample.py
```

#### 从远程机器桌面上复制文件夹到本地上

```sh
scp -P port -r user@remote:Desktop/sample ~/Desktop/sample
```

## ssh 高级用法

### 免密码登录

免密码登录：即客户端访问服务端时，需要密码验证身份登录。

- Step.01. 配置公钥：执行 ssh-keygen 即生成 SSH 密钥。
- Step.02. 上传公钥到服务器：执行 ssh-copy-id -p port user@remote，让远程服务器记住我们的 公钥。可能需要使用 -i 指定公钥位置：ssh-copy-id -i ~/.ssh/id_rsa.pub user@remote.

> 1) 有关 SSH 配置信息都保存在 /Home/yousr username/.ssh 目录下。
> 2) 免密登录使用的是非对称加密算法 ( RSA )，即使用公钥加密的数据，需要使用私钥解密；使用私钥加密的数据，需要使用公钥解密。

### 配置别名

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

# shell 相关操作

## bash

`bash` 是大多数 linux 系统默认的登录 shell。由于 `zshrc` 是兼容 `bash` 的，因此配置的相关内容统一放到介绍 `zshrc` 的时候。这里主要介绍一下几个常用的 `bash` 配置文件。

```text
/bin/bash
       The bash executable
/etc/profile
       The systemwide initialization file, executed for login shells
~/.bash_profile
       The personal initialization file, executed for login shells
~/.bashrc
       The individual per-interactive-shell startup file
~/.bash_logout
       The individual login shell cleanup file, executed when a login shell exits
~/.inputrc
       Individual readline initialization file
```

## [fish](http://fishshell.com/)

[github-repository](https://github.com/fish-shell/fish-shell)

### 安装

- ubuntu

```sh
sudo apt-add-repository ppa:fish-shell/release-3
sudo apt-get update
sudo apt-get install fish
```

### 配置

配置非常方便，先在命令行中输入 `fish` 切换shell为 `fish` ，然后图形界面下会显示一个http的链接，点击进入该链接进行相应配置即可。

```sh
fish_config
```

{% asset_img fish_config.png %}

基本上 `fish` 是一个开箱即用的shell，几乎不进行配置即可使用，但是 `fish` 和 `bash` 不兼容，所以稍加配置的 `zsh` 是更好的选择。

## zsh

ubuntu 下安装：

```sh
$ sudo apt install zsh
```

修改默认登录 shell

```sh
# 查看默认 shell
$ cat /etc/shells
# /etc/shells: valid login shells
/bin/sh
/bin/bash
/bin/rbash
/bin/dash
/usr/bin/tmux
/usr/bin/fish
/bin/zsh
/usr/bin/zsh

# 切换登录 shell 为 zsh
$ chsh -s /bin/zsh

# 可以通过 echo $SHELL 查看当前默认的 Shell，如果没有改为 /bin/zsh，那么需要重启 Shell。
```

安装 Oh My Zsh

```sh
# 安装 Oh My Zsh
wget https://github.com/robbyrussell/oh-my-zsh/raw/master/tools/install.sh -O - | sh
# 以上命令可能不好使，可使用如下两条命令
wget https://github.com/robbyrussell/oh-my-zsh/raw/master/tools/install.sh
bash ./install.sh
```

安装插件

在 `.zshrc` 中配置要安装的插件：

```sh
# Which plugins would you like to load?
# Standard plugins can be found in ~/.oh-my-zsh/plugins/*
# Custom plugins may be added to ~/.oh-my-zsh/custom/plugins/
# Example format: plugins=(rails git textmate ruby lighthouse)
# Add wisely, as too many plugins slow down shell startup.
plugins=(
  git 
  git-open
  vi-mode
  extract
  zsh-autosuggestions
  zsh-syntax-highlighting
  web-search
  autojump
  wd
)
```

```sh
# git open
npm install --global git-open

# autojump
git clone git://github.com/wting/autojump.git
cd autojump
./install.py or ./uninstall.py

# add following line to .zshrc
[[ -s /home/username/.autojump/etc/profile.d/autojump.sh ]] && source /home/username/.autojump/etc/profile.d/autojump.sh
autoload -U compinit && compinit -u
```

遇到的问题

```sh
Warning: plugin zsh-syntax-highlighting not found
Warning: plugin zsh-autosuggestions not found

# I solve it by the command :
git clone https://github.com/zsh-users/zsh-autosuggestions ~/.oh-my-zsh/custom/plugins/zsh-autosuggestions
git clone https://github.com/zsh-users/zsh-syntax-highlighting.git ~/.oh-my-zsh/custom/plugins/zsh-syntax-highlighting

# on-my-zsh check if the plugin is exit at '$base_dir/plugins/$name/$name.plugin.zsh' or '$base_dir/plugins/$name/_$name' , so the plugin should be at this path .
```

## xonsh

# git 相关操作

## git 新建仓库并推送到远程分支

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

# 工具部分

## 命令行辅助

### [tldr](http://tldr-pages.github.io/)

[github-repository](https://github.com/tldr-pages/tldr)

> A collection of simplified and community-driven man pages.

{% asset_img tldr.png %}

`github`主页提供了多种客户端实现，这里介绍`python`和`c++`客户端：

- python

```sh
tldr-python-client: pip install tldr
tldr.py: pip install tldr.py
```

`python`客户端安装简易，但是性能好像有些问题，查询速度很慢。

- c++

`c++`客户端通过源码编译的方式进行安装。

```sh
git clone https://github.com/tldr-pages/tldr-cpp-client.git tldr-c-client
cd tldr-c-client

./deps.sh           # install dependencies
make                # build tldr
make install        # install tldr
```

性能很棒。

### [cheat](https://github.com/cheat/cheat)

作用类似于 tldr，不过更具有定制性，通过使用它可以制作一套属于自己的命令提示工具。

使用 `pip` 安装即可：

```sh
[sudo] pip install cheat
```

使用方法：

```sh
cheat

Create and view cheatsheets on the command line.

Usage:
  cheat <cheatsheet>
  cheat -e <cheatsheet>
  cheat -s <keyword>
  cheat -l
  cheat -d
  cheat -v

Options:
  -d --directories  List directories on $CHEAT_PATH
  -e --edit         Edit cheatsheet
  -l --list         List cheatsheets
  -s --search       Search cheatsheets for <keyword>
  -v --version      Print the version number

Examples:

  To view the `tar` cheatsheet:
    cheat tar

  To edit (or create) the `foo` cheatsheet:
    cheat -e foo

  To list all available cheatsheets:
    cheat -l

  To search for "ssh" among all cheatsheets:
    cheat -s ssh

```

### [tig](https://github.com/jonas/tig)

命令行界面的 git 客户端。

通过源码编译安装：

```sh
$ git clone http://github.com/jonas/tig
$ cd tig
$ make prefix=/usr/local
...
include/tig/tig.h:93:22: fatal error: curses.h: No such file or directory
compilation terminated.
Makefile:334: recipe for target 'src/tig.o' failed
make: *** [src/tig.o] Error 1

$ sudo apt-get install libncurses5-dev libncursesw5-dev
$ make prefix=/usr/local
$ sudo make install prefix=/usr/local() sudo make install prefix=/usr/local
```

## 网络辅助工具

### [proxychains-ng](https://github.com/rofl0r/proxychains-ng)

这个工具使用 `HOOK` 技术使大多数的命令行工具可以走 socks 或者 http(s) 代理。

安装 proxychains-ng：

```sh
$ git clone https://github.com/rofl0r/proxychains-ng.git
$ cd proxychains-ng
$ ./configure --prefix=/usr --sysconfdir=/etc
$ sudo make install  # install proxychains4
$ proxychains4

Usage:  proxychains4 -q -f config_file program_name [arguments]
        -q makes proxychains quiet - this overrides the config setting
        -f allows one to manually specify a configfile to use
        for example : proxychains telnet somehost.com
More help in README file
```

配置：

```sh
$ sudo make install-config  # install proxychains.conf (/etc/proxychains.conf)
$ sudo vim /etc/proxychains.conf

[ProxyList]
# add proxy here ...
# meanwile
# defaults set to "tor"
# socks4        127.0.0.1 9050
socks5  127.0.0.1 1080

$ vim ~/.bashrc

alias pc="proxychains4" 

$ source ~/.bashrc
```

使用：

```sh
$ pc sudo apt install thefuck
$ pc wget bing.com
```

### [syncthing](https://github.com/syncthing/syncthing)

一个跨平台的分布式同步工具。

安装：

```sh
# for ubuntu
# Add the release PGP keys:
$ curl -s https://syncthing.net/release-key.txt | sudo apt-key add -

# Add the "stable" channel to your APT sources:
$ echo "deb https://apt.syncthing.net/ syncthing stable" | sudo tee /etc/apt/sources.list.d/syncthing.list

# Update and install syncthing:
$ sudo apt-get update
$ sudo apt-get install syncthing
```

## Blog 相关

### [nvm](https://github.com/nvm-sh/nvm)

Node 版本管理工具——一个简单的 bash 脚本，用于管理多个活动的 Node.js 版本。

安装和更新：

```sh
$ curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.34.0/install.sh | bash

# or wget
$ wget -qO- https://raw.githubusercontent.com/nvm-sh/nvm/v0.34.0/install.sh | bash

# or ansible playbook
- name: nvm
  shell: >
    curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.34.0/install.sh | bash
  args:
    creates: "{{ ansible_env.HOME }}/.nvm/nvm.sh"
```

使用：

```sh
# install node
$ nvm install node # "node" is an alias for the latest version
$ nvm install 6.14.4 # or 10.10.0, 8.9.1, etc

# list available versionss
$ nvm ls-remote

# use installed version
$ nvm use node
```

### [cnpm](https://npm.taobao.org/)

淘宝的 npm 镜像。

安装：

```sh
$ npm install -g cnpm --registry=https://registry.npm.taobao.org

# or alias it in .bashrc or .zshrc
$ echo '\n#alias for cnpm\nalias cnpm="npm --registry=https://registry.npm.taobao.org \
  --cache=$HOME/.npm/.cache/cnpm \
  --disturl=https://npm.taobao.org/dist \
  --userconfig=$HOME/.cnpmrc"' >> ~/.zshrc && source ~/.zshrc
```

使用：

```sh
$ npm install [name]
```

### [hexo](https://hexo.io/zh-cn/)

一个快速、简洁和高效的博客框架。

安装和使用：

```sh
$ npm install hexo-cli -g
$ hexo init blog
$ cd blog
$ npm install
$ hexo server
```

# 参考

[ubuntu 下给用户添加 sudo 权限，并且如何取消 sudo 权限](https://blog.csdn.net/u011774239/article/details/48463393)  
[linux 下创建用户并且限定用户主目录](http://blog.sina.com.cn/s/blog_47051c800100oegn.html)
[几种 bash 配置文件](https://stackoverflow.com/a/416931)
[Linux Error: curses.h: No such file or directory Problem Solution](https://www.cyberciti.biz/faq/linux-error-cursesh-no-such-file-directory/)
[Warning: plugin zsh-syntax-highlighting not found #7688](https://github.com/robbyrussell/oh-my-zsh/issues/7688)