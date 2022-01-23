---
title: "Linux服务器内核优化"
date: 2022-01-10 23:29:53
categories:
- Linux
tags:
- debian
- kernel 
- 优化
---
确认内核版本在3.5+

编辑sysctl.conf

```
vim /etc/sysctl.conf
```

写入

```
net.core.default_qdisc = fq
net.ipv4.tcp_congestion_control = bbr
fs.file-max = 65535
net.ipv4.tcp_timestamps = 1
net.ipv4.tcp_tw_reuse = 1
net.ipv4.tcp_fin_timeout = 30
net.ipv4.tcp_keepalive_time = 600
net.ipv4.ip_local_port_range = 10000 65000
net.ipv4.tcp_max_syn_backlog = 4096
net.ipv4.tcp_max_tw_buckets = 5000
net.ipv4.tcp_rmem = 4096 87380 67108864
net.ipv4.tcp_wmem = 4096 65536 67108864
net.ipv4.tcp_mtu_probing = 1
```

输入以下命令让其生效

```
sysctl --system
sysctl -p
```

提高最大可打开文件描述符数量

编辑limits.conf

```
vim /etc/security/limits.conf
```

写入: 
(root账户将*替换为root)

```
* soft nofile 65535
* hard nofile 65535
```

设置ulimit

```
ulimit -n 65535
```
