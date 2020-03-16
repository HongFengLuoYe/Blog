---
title: VPS
date: 2020-03-03 21:27:17
tags: VPN
category: 网络技术
archive: 2020
---
之前一直在用一个国外翻墙软件，一直想自己搭建一个自己vps服务器用于写代码😃，偶尔一次看到可以使用这个视频，来学习下，怎么自己搭建SSR。
# 搭建SSR
[视频链接](https://www.youtube.com/watch?v=RxbGtkRVUWQ)
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




-------




# Trojan安装
 用了一段是时间的SSR出现了TCP阻断，为了防止出现不能访问国外网站的事情，所以特意来搭建一下Trojan。视频教程还是来自[番茄Style](https://www.youtube.com/watch?v=hYfaaTdfB8s)。
## VPS
### 创建实例
![](vps/Snip20200316_27.png)
### 检测IP
![](vps/Snip20200316_28.png)
## 申请域名
### 购买域名
![](vps/Snip20200316_29.png)
![](vps/Snip20200316_30.png)
![](vps/Snip20200316_31.png)
![](vps/Snip20200316_32.png)

### 域名托管
![](vps/Snip20200316_33.png)
![](vps/Snip20200316_34.png)
![](vps/Snip20200316_35.png)
![](vps/Snip20200316_37.png)
## 解析域名
### 域名解析
![](vps/Snip20200316_38.png)
![](vps/Snip20200316_42.png)
![](vps/Snip20200316_43.png)
### 测试IP
![](vps/Snip20200316_39.png)
## 安装
### 进入BBRPlus
* 进入管理员：

```
sudo -i
```
* 查看当前目录的脚本：

```
root@ssr:~# ls
ssr.sh  tcp.sh  trojan_mult.sh

```

### 安装Trojan
* 复制以下命令在VPS中执行，选择安装trojan，然后输入解析到VPS的域名并回车（不要带http://，只输入域名，例如atrandys.com或者xxx.atrandys.com），开始安装，然后等待安装完成即可。

* 注意：如果提示SELinux状态问题，请按要求输入Y重启VPS，然后再执行本脚本，否则可能https证书申请出错


```
curl -O https://raw.githubusercontent.com/atrandys/trojan/master/trojan_mult.sh && chmod +x trojan_mult.sh && ./trojan_mult.sh

```
![](vps/Snip20200316_40.png)
![](vps/Snip20200316_41.png)
![](vps/Snip20200316_44.png)

## 客户端设置
### Mac
Mac电脑下，可以通过终端工具和浏览器使用Trojan。
#### 插件
![](vps/Snip20200316_46.png)
[https://github.com/trojan-gfw/trojan/releases/tag/v1.14.1](https://github.com/trojan-gfw/trojan/releases/tag/v1.14.1)
#### 终端
* 浏览器可以上网了，终端还不可以，参考了两篇博文：
> [1.MAC终端代理设置](https://blog.csdn.net/Lengwenin/article/details/104425956)
> [2.让终端走代理的几种方法](https://blog.fazero.me/2015/09/15/%E8%AE%A9%E7%BB%88%E7%AB%AF%E8%B5%B0%E4%BB%A3%E7%90%86%E7%9A%84%E5%87%A0%E7%A7%8D%E6%96%B9%E6%B3%95/)。
* 总结出以下代码：

```
#设置环境变量

dscl . -read /Users/$USER UserShell

vim ~/.bash_profile



#编辑bash_profile


添加:
function ssr_off(){
        unset http_proxy
        unset https_proxy
        unset ftp_proxy
        unset rsync_proxy
        echo -e "已关闭ssr"
}
 
function ssr_on() {
        export no_proxy="localhost,127.0.0.1,localaddress,.localdomain.com"
        export http_proxy="http://127.0.0.1:1087"
        export https_proxy=$http_proxy
        export ftp_proxy=$http_proxy
        export rsync_proxy=$http_proxy
        export HTTP_PROXY=$http_proxy
        export HTTPS_PROXY=$http_proxy
        export FTP_PROXY=$http_proxy
        export RSYNC_PROXY=$http_proxy
        echo -e "已开启ssr"
}
function torjan_off(){
        unset ALL_PROXY
        echo -e "已关闭trojan"
}
 
function trojan_on() {
        export ALL_PROXY=socks5://127.0.0.1:1080
        echo -e "已开启trojan"
}


#执行bash_profile
source ~/.bash_profile


#在任何中立执行
ssr_on
ssr_off
trojan_on
torjan_off

```

### iOS
小火箭shadowrocket
![](vps/Snip20200316_47.png)

