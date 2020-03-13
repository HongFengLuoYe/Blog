---
title: 博客工具Hexo的安装、使用和部署
date: 2020-03-04 08:39:32
tags: Blog
archive: 2020
category: 知识整理
---

之前，因为工作原因需要将许多工作和学习的资料保存下来，供日后使用，在选择各种笔记工具时比较犯难，所以想把这些东西写下来，做保留。所以想到用博客记录下，hexo算是目前比较好的博客工具。此次，只是为了记录安装过程。简化了很多流程。如果需要学习的童鞋：请转至 **[教程网址](https://www.youtube.com/watch?v=PsXWbI2Mqu0)**

# 安装使用
## 准备工作
### 安装Node.js

>Node.js 为大多数平台提供了官方的安装程序:https://nodejs.org/en/download/

### 安装Git
> 下载 安装程序:https://sourceforge.net/projects/git-osx-installer/


## 安装hexo
所有必备的应用程序安装完成后，即可使用 npm 安装 Hexo。
```
$ npm install -g hexo-cli
```
安装 Hexo 完成后，请执行下列命令，Hexo 将会在指定文件夹中新建所需要的文件。

```
$ hexo init <folder>
$ cd <folder>
$ npm install
```

### 新建md文件

```
$ hexo new [layout] <title>
```
新建一篇文章。如果没有设置 layout 的话，默认使用 _config.yml 中的 default_layout 参数代替。如果标题包含空格的话，请使用引号括起来。例如：

```
$ hexo new "post title with whitespace"
```

### 生成静态网站
```
$ hexo generate
```
生成静态文件。
```
$ hexo server
```
启动服务器。默认情况下，访问网址为： http://localhost:4000/。

### 开启本地服务和调试
可以同时开启服务和监控生成静态文件服务，用于调试。
```
$ hexo generate -w
```
再开一个终端窗口
```
$ hexo server
```
然后可以通过Mweb编辑，在浏览器中查看效果。

# 上传GitHub
## 创建hexo一键部署
安装hexo-deployer-git

```
$ npm install hexo-deployer-git --save
```


如果不确定有没有hexo-deployer-git，可以

```
$ nmp list hexo-deployer-git
```
修改配置

```
deploy:
  type: git
  repo: <repository url> #https://bitbucket.org/JohnSmith/johnsmith.bitbucket.io
  branch: [branch]
  message: [message]
```

## 创建仓库
创建新的仓库，如果需要通过GitHub Page访问网站，创建仓库时候必须符合这样的规则：https://pages.github.com/
## 通过ssh部署代码

生成ssh key

```
$ ssh
```
![](hexo-blog/Snip20200304_19.jpg)

```
$ ssh-keygan -t rsa
```
(千万不要输密码)
![生成秘钥](hexo-blog/16328000.png)

生成两个文件，id_rsa.pub公钥，id_rsa私钥,查看文件：


```
$ cd .ssh
$ ls
$ cat id_rsa.pub
```
将公钥添加GitHub上
![](hexo-blog/163280001.png)

测试添加成功

```
$ ssh -T git@github.com
```
![](hexo-blog/163280002.png)
最后实现一键部署到GitHub
```
$ hexo d
```

# 更换主题
## 安装主题
看了好多主题，最后还是参考了[唐巧](https://blog.devtang.com/)的博客，既有分类和也很美观。**archer主题** 参考下面网址：
```
https://github.com/fi3ework/hexo-theme-archer
```
## 自定义元素
**看下面哦**😃
```
https://github.com/fi3ework/hexo-theme-archer
```
## 本地图片加载
进入博客目录，安装该插件。

```
npm install https://github.com/CodeFalling/hexo-asset-image –save
```

修改_config.yml文件，post_asset_folder:改为true。

```
post_asset_folder: true
```
完成后新建md会生成同名文件，在md中引用图片就行。
![](hexo-blog/Snip20200305_22.png)

例如上面这个图片就是引进去的

```
![](hexo-blog/Snip20200305_22.png)
```

# 部署到阿里云
先搞明白Hexo博客从搭建到自动发布的架构，才能更好的理解我们每一步进行的操作。不然只跟着步骤过了一遍，却不知道为什么这么做。
![](hexo-blog/nginx-aliyun.png)

好了，原理懂了，我们说下我做的思路：1.从GitHub克隆过来我的仓库到网站根目录；2.新建Git仓库，并设置git-hooks；3.配置ssh公钥和hexo，一键部署。下面我们就开始吧：

> 环境：阿里云 ubuntu16.04

## 克隆代码

### 安装Nginx和Git
```
apt-get update
apt-get install nginx
apt-get install git
```
配置好全局 git 的用户名和邮箱，如下：

```
git config --global user.name "姓名"
git config --global user.email "邮箱"
```
### 从GitHub克隆代码
现在我们进入`/var/www/html`中，clone下之前托管在 GitHub上的博客源代码。
```
git clone https://github.com/HongFengLuoYe/HongFengLuoYe.github.io.git
```

### 配置Nginx根目录
前面说了，nginx 默认的目录为 `/var/www/html`，我们将我们的博客的源代码 clone 至此，并修改 `/etc/nginx/sites-available/default`，将 server 下的 root 字段值修改为 clone 后的博客目录路径。


```
server {
    listen 80 default_server;
    listen [::]:80 default_server;
    root /var/www/html/HongFengLuoYe.github.io;
    }
```

我的博客在 GitHub 上的仓库名为 `https://github.com/HongFengLuoYe/HongFengLuoYe.github.io.git`，所以修改为:

```
 root /var/www/html/HongFengLuoYe.github.io;
```

这个时候，我们即使没有备案，通过 IP 也可以访问自己的博客，可以直接在浏览器的地址栏输入阿里云公网的 IP 访问自己的博客。如果通过公网 IP 访问没有问题，说明环境搭建成功。

最后重启Nginx
```
systemctl restart nginx
```

## 创建Git钩子
虽然看起来我们似乎将原来托管在GitHub迁移到阿里云服务器上了，但是博客源代码是通过手动`git clone`下来到`/var/www/html/HongFengLuoYe.github.io`上的，如果我们需要对博客进行修改或者发布新的文章，运行`hexo d`还是将会更新的博客源代码上传至GitHub上，并没有自动上传到阿里云的服务器上。如果想同步修改，解决方法很简单，在阿里云的机器上搭建一个Git远程仓库（相对本地仓库来说），像GitHub一样，每次通过`hexo d` 时候，也将网站内容更新到阿里云的Git仓库中，并自动同步到`/var/www/html/HongFengLuoYe.github.io`中，这就是上图所示的原理。做法如下：


### 创建git用户

```
adduser git
```
在`/home/git`目录下创建裸仓 hexo.git：


```
cd /home/git
git init --bare hexo.git
```

### 修改用户权限：

```
cd ..
sudo chmod -R 777 home #为了操作方便我把home都设置了最高权限
```

## 配置阿里云和一键部署

### 配置SSH公钥：
```
cat ~/.ssh/id_rsa.pub
```
复制公钥，将公钥写入阿里云机器的 /home/git/.ssh/authorized_keys 文件中
```
vim .ssh/authorized_keys #如果没有authorized_keys就自己创建一个
```

### 设置Hooks

在`/home/git/hexo/hooks/post-receive`文件中写入：


```
#!/bin/sh
git --work-tree=/var/www/html/HongFengLuoYe.github.io --git-dir=/home/git/hexo.git checkout -f
```
修改权限`/var/www/html`文件夹权限，以免访问时候没有权限。

```
sudo chmod -R 777 www
```

### 修改本地配置
修改本地机器上的 _config.yml 文件，在 deploy 块中添加阿里云机器上刚创建的 hexo.git 仓库：

```
deploy:
  type: git
  repo: 
    github: git@github.com:HongFengLuoYe/HongFengLuoYe.github.io.git
    hexo: git@47.104.153.138:/home/git/hexo.git,master
```

进入博客一键部署

```
hexo d
```
#  高级功能
## 实时刷新
### 下载LiveReload
下载[Mac客户端](http://livereload.com/)，安装Chrome插件。
![](hexo-blog/Snip20200307_28.png)
### 添加文件夹
添加要监控的文件夹，开启web服务，打开网页，点击插件把图标变成黑点，编辑文本时只要保存（CTRL+S）后会实时刷新页面。
![](hexo-blog/Snip20200307_24.png)
![](hexo-blog/Snip20200307_25.png)
![](hexo-blog/Snip20200307_27.png)

## 博客加密

> 参考： https://github.com/MikeCoder/hexo-blog-encrypt

Hexo会将我们本地编写好的markdown文章进行编译，然后生成静态页面部署到服务器之中。

但有些文章我们不希望公开，希望将文章进行加密，用户只有输入正确的验证码之后，才能进行访问。

这种情况，wordpress、emlog等其他博客系统很容易进行设置，但是Hexo却不行。

为了解决这个需求，我们需要引入`hexo-blog-encrypt` 。

### 特性
* 一旦你输入了正确的密码, 它将会被存储在本地浏览器的localStorage中。再次访问，不需输入密码。

* 支持按标签加密。

* 所有的核心功能都是由原生的API所提供的。 在 Node.js中, 我们使用 Crypto。在浏览器中, 我们使用 Web Crypto API。

* PBKDF2, SHA256 被用于分发密钥, AES256-CBC 被用于加解密, 还使用 HMAC 来验证密文的来源, 并确保其未被篡改。

* 广泛地使用 Promise 来进行异步操作, 以此确保线程不被杜塞。

* 过时的浏览器将不能正常显示, 因此, 请升级浏览器。

### 安装

```
npm install --save hexo-blog-encrypt

```
### 快速使用
将 “password” 字段添加到文章信息头：


```
title: Hello World
date: 2016-03-30 21:18:02
password: abcd1234xxx
```
---
再使用`hexo clean && hexo g && hexo s`在本地预览加密的文章。




### 文章信息头

```
title: Hello World
tags:
- 加密文章tag
date: 2019-10-26 20:22:58
password: mikemessi
abstract: 该文章已加密, 请输入密码查看。
message: 该文章已加密, 请输入密码查看。
wrong_pass_message: 密码不正确，请重新输入！
wrong_hash_message: 文章不能被校验, 不过您还是能看看解密后的内容！
```

### _config.yml
示例

```
encrypt: # hexo-blog-encrypt
  abstract: 有东西被加密了, 请输入密码查看.
  message: 您好, 这里需要密码.
  tags:
  - {name: tagName, password: 密码A}
  - {name: tagName, password: 密码B}
  template: <div id="hexo-blog-encrypt" data-wpm="{{hbeWrongPassMessage}}" data-whm="{{hbeWrongHashMessage}}"><div class="hbe-input-container"><input type="password" id="hbePass" placeholder="{{hbeMessage}}" /><label>{{hbeMessage}}</label><div class="bottom-line"></div></div><script id="hbeData" type="hbeData" data-hmacdigest="{{hbeHmacDigest}}">{{hbeEncryptedData}}</script></div>
  wrong_pass_message: 抱歉, 这个密码看着不太对, 请再试试.
  wrong_hash_message: 抱歉, 这个文章不能被校验, 不过您还是能看看解密后的内容.
```
注：若文章中采用_config.yml中的全局配置，则文章的tags只能有一个，若有多个tags，则失效。

### 配置优先级
文章信息头 > _config.yml (站点根目录下的) > 默认配置

## 定制版权页
效果：HFY导航到GitHub主页，原文链接设置成GitHub Page页。发表日期和更新日期不可点击。
![](hexo-blog/Snip20200308_30.png)
1.配置hexo的配置信息。
![](hexo-blog/Snip20200308_31.png)
2.配置archer主题配置信息。
![](hexo-blog/Snip20200308_32.png)
3.设置post.ejs。
![](hexo-blog/Snip20200308_33.png)

具体信息参考：[**HFY主页**](https://github.com/HongFengLuoYe/HongFengLuoYe.github.io)

> **感谢以下博客：**
> [leehoward](https://leehoward.cn/)
> [howarzheng](https://blog.csdn.net/qq_34272417/article/details/99491294)
> [青草_e75f](https://www.jianshu.com/p/bcfde206b29a)
