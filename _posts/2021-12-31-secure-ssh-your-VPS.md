---
title: "怎样安全的设置你的VPS"
date: 2021-12-31 23:29:53
categories:
- Linux
tags:
- 安全
- ssh 
- vps
---
服务商预装的系统可能预装了用户行为日志记录程序,
而且服务商并没有自定义iso系统安装的功能, 
这时我们就可以dd重装系统。

首先 更新系统

apt update && apt upgrade -y

安装依赖

```
apt install wget xz-utils openssl gawk file
```

下载脚本赋予脚本执行权限

```
wget https://cdn.jsdelivr.net/gh/isora0/script@master/DebianNET.sh
chmod a+x DebianNET.sh
```

运行脚本(-d 10 -v 64/debian 10 64位 | -p /自定义root密码 | -a/自动模式) 
==默认ssh端口3022==

```
bash DebianNET.sh -d 10 -v 64 -p 2DOiSora2020= -a
```

等待系统安装完毕 ssh登录

更新dd后的系统

apt update && apt upgrade -y

安装vim 有非root账户登录习惯的安装sudo
 
```
apt install vim sudo
```

设置时区为上海

```
timedatectl set-timezone Asia/Shanghai
```

修改dns并固化

```
vim /etc/resolv.conf
chattr +i /etc/resolv.conf
```

添加新用户xx 

```
adduser xx
```

设置sudo 

sudo免密码切root账户: xx ALL=(ALL) NOPASSWD: ALL)

```
chmod +w /etc/sudoers
vim /etc/sudoers
chmod -w /etc/sudoers
```

设置ssh密钥登录

生成密钥对

```
ssh-keygen -t rsa -b 1024
```

将公钥添加进信任列表，更改权限

记得导出私钥

```
cat >> .ssh/authorized_keys' < ~/.ssh/id_rsa.pub
chmod 600 ~/.ssh/authorized_keys
```

修改ssh配置 修改ssh端口 禁止root登录 ......

```
vim /etc/ssh/sshd_config
```

安装配置UFW 允许ssh端口

```
apt install ufw
ufw default deny incoming
ufw default allow outgoing
ufw allow 3022
ufw enable
```

