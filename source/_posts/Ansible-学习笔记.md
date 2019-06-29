---
title: Ansible 学习笔记
date: 2019-05-28 20:35:32
categorises:
- 学习笔记
tags:
- ansible
- python
---

## Ansible 介绍

Ansible 是一款运维工具，它基于 ssh，因此不需要在远程机器上安装额外的客户端软件。

## Ansible 使用入门

### 安装 Ansible

```sh
pip install ansible
```

Ansible 依赖 Python 和 ssh，服务器端需要安装 ssh 和版本号大于 2.5 的 Python。Ansible 安装完成后，控制端会增加以下几个可执行程序：

- ansible
- ansible-doc
- ansible-playbook
- ansible-vault
- ansible-console
- ansible-galaxy
- ansible-pull

### Ansible 的运行环境

在 /etc/ansible/hosts 文件中配置远程主机的域名或 ip 地址，Ansible 会默认读取该文件获取远程主机列表。

```sh
[test]
127.0.0.1
10.102.18.12
```

还可以在该 hosts 文件中配置用户名和端口号：

```sh
[test]
127.0.0.1 ansible_user=root ansible_port=2222
10.102.18.12 ansible_user=root ansible_port=2222
```

或者在配置文件中对 Ansible 的行为进行配置，配置文件将按照下列顺序进行搜索：

- ANSIBLE_CONFIG (environment variable if set)
- ansible.cfg (in the current directory)
- ~/.ansible.cfg (in the home directory)
- /etc/ansible/ansible.cfg

Ansible 会解析上述列表，并使用它找到的第一个配置文件，其他的将会被忽略。

```sh
[defaults]
remote_port=2222
remote_user=root
```

### Ansible 的 ad-hoc 模式

```sh
ansible test -m ping  # 测试 ssh 连接
ansible test -m command -a "ps"  # 远程执行 ps 命令
```

### 使用 playbook 控制服务器

使用YAML配置文件编写任务。编写如下 playbook，命名为 test_playbook.yml。

```yaml
---
- host: test
  tasks:
  - name: copy file
    copy: src=/tmp/data.txt dest=/tmp/data.txt

  - name: change mode
    file: dest=/tmp/data.txt mode=500 owner=root group=root

  - name: ensure packages installed
    apt: pkg={{ item }} state=present
    with_items:
      - tmux
      - git
```

然后使用 `ansible-playbook test_playbook.yml` 执行即可。