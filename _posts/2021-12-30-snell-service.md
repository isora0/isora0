---
title: "snell代理服务端搭建"
date: 2021-12-30 23:29:53
categories:
- Linux
tags:
- surge
- snell 
- 代理
---
snell是知名网络调试应用surge团队开发的加密代理程序。

[项目地址](https://github.com/surge-networks/snell) 目前版本3.0后开始支持UDP转发。我们以AMD64位Debian系统为例开始服务端搭建。

确认服务器已安装好wget 和unzip 。

wget下载项目release

    wget https://github.com/surge-networks/snell/releases/download/v3.0.0/snell-server-v3.0.0-linux-amd64.zip

解压下载的release

    unzip snell-server-v3.0.0-linux-amd64.zip

运行程序，弹出对话输入y，生成配置文件

    ./snell-server

编辑配置文件，重置端口号和密码

    vim snell-server.conf

移动snell主程序和配置文件到系统以下目录

    mv snell-server /usr/local/bin/
    mv snell-server.conf /etc/

编辑systemd自启动文件

    vim /lib/systemd/system/snell.service

写入如下内容

    [Unit]
    Description=Snell Proxy Service
    After=network.target

    [Service]
    Type=simple
    User=nobody
    Group=nogroup
    LimitNOFILE=65535
    ExecStart=/usr/local/bin/snell-server -c /etc/snell-server.conf

    [Install]
    WantedBy=multi-user.target

设置程序自启动

    systemctl daemon-reload
    systemctl enable snell.service
    systemctl start snell.service

确认服务启动成功

    systemctl status snell.service
    
snell服务端就搭建完成,配合支持snell的客户端使用即可。
