---
title: "Linux VPS xmrig零抽水挖狗狗币"
date: 2022-01-15 23:29:53
categories:
- Linux
tags:
- 狗狗币
- 挖矿
- xmrig 
---
xmrig是一款很好用的开源挖矿程序。

[项目地址](https://github.com/xmrig/xmrig)

软件默认有1%抽水，我们可以重新编译修改抽水比例为0

开始编译:

安装依赖:

```
apt install git build-essential cmake libuv1-dev libssl-dev libhwloc-dev -y
```

```
apt install automake libtool autoconf -y
```

拉取源码:

```
git clone https://github.com/xmrig/xmrig.git
```

修改默认捐赠比例和最小捐赠比例的值1 为0

```
vim xmrig/src/donate.h
```

![](https://s4.ax1x.com/2022/01/15/7Ym37Q.png)

处理静态依赖:

```
cd xmrig/scripts && ./build_deps.sh
```

编译:

```
cd .. && mkdir build && cd build
cmake .. -DXMRIG_DEPS=scripts/deps
make -j$(nproc)
```

拥有自己的狗狗币钱包地址

寻找矿池 这里推荐矿池: [矿池地址](https://www.unmineable.com/)
提币费用小至0.75%

运行xmrig:

将钱包地址替换为自己狗币钱包地址
将百分比数值设置为合适数值


```
cd xmrig/build && ./xmrig -o rx.unmineable.com:3333 -a rx -k -B -u DOGE:钱包地址.矿工名#qmsf-w9cr 
```

软件默认满cpu挖矿 我们可以安装cpulimit限制cpu占用

cpulimit -e xmrig -l 50

安装screen可让软件保持后台运行
