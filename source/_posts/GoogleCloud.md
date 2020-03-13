---
title: 谷歌云VPS搭建SSR
date: 2020-03-03 21:27:17
tags: VPN
category: 网络技术
archive: 2020
---
之前一直在用一个国外翻墙软件，一直想自己搭建一个自己vps服务器用于写代码😃，偶尔一次看到可以使用这个视频，来学习下，怎么自己搭建SSR。
# 谷歌云VPS搭建SSR
## [视频链接](https://www.youtube.com/watch?v=RxbGtkRVUWQ)
> ►谷歌云地址：
> https://cloud.google.com/
> ►Ping检测：
> http://ping.chinaz.com
> ►VPS连接软件（PuTTY）：
> https://www.lanzous.com/i68xjpg
> ►国外地址生成：
> https://www.dizhishengcheng.com/

## 1.注册谷歌云
SSH客户端登录设置：
* 输入：`vi /etc/ssh/sshd_config`
* 键盘输入 `i`，找到`PermitRootLogin no`，将`no`改为`yes`；
* 再找到`PasswordAuthenticaion no`，将`no`改为`yes`；
* 键盘按`Esc`，再输入 `:wq`
* 输入：`passwd`，设置密码（设置并确认）
* 输入：`systemctl restart sshd`

## 2.安装BBRPlus

更换内核需要root权限，所以如果你是普通用户的话，需要root账号权限，如果你是root账号，那就忽略这个步骤：

```
sudo -i
```
BBR一键安装代码：

```
wget --no-check-certificate https://raw.githubusercontent.com/cx9208/Linux-NetSpeed/master/tcp.sh && chmod +x tcp.sh && ./tcp.sh
```

若报错运行以下代码，不报错跳过：

```
yum -y install wget
rm -f /var/run/yum.pid
```
重新打开脚本，查看是否运行成功：

```
./tcp.sh
```
## 3.安装ShadowsocksR
简单的来说，如果你什么都不懂，那么你直接一路回车就可以了！

1、本脚本需要Linux root账户权限才能正常安装运行，所以如果不是 root账号，请先切换为root，如果是 root账号，那么请跳过！

```
sudo -i
```

2、ShadowsocksR服务端一键安装：

```
wget -N --no-check-certificate https://raw.githubusercontent.com/ToyoDAdoubiBackup/doubi/master/ssr.sh && chmod +x ssr.sh && bash ssr.sh
```

运行脚本，输入对应的数字来执行相应的命令。

```
bash ssr.sh
```