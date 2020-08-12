---
title: Java
date: 2020-08-11 07:30:31
tags: Back-end
archive:
category: 计算机
---
# 环境搭建
![](java/024f78f0f736afc3646ddea1b419ebc4b6451294.jpeg)

获取系统属性,查看文件编码格式

```
System.getProperties().list(System.out);
```

在CMD中更改为UTF8

```
chcp 65001
```

通过系统环境变量，设置Java编码格式
![iRE Java JDK ARmy UTF-8 - Java Fe](media/iRE%20Java%20JDK%20ARmy%20UTF-8%20-%20Java%20Fe.png)
变量名为: JAVA_TOOL_OPTIONS， 变量值为：-Dfile.encoding=UTF-8

# 绘制窗体
![创建窗体](media/%E5%88%9B%E5%BB%BA%E7%AA%97%E4%BD%93.png)
![](media/15971028858185.jpg)

# 绘制乌龟
![绘制乌龟](media/%E7%BB%98%E5%88%B6%E4%B9%8C%E9%BE%9F.png)

# idea配置git
[链接](https://www.cnblogs.com/aimei/p/12721419.html)

## GitHub Token
![](http://upload-images.jianshu.io/upload_images/16375643-97d86d01e1aeafbe.png)

选择左侧导航  Developer settings (开发人员设置)

![](http://upload-images.jianshu.io/upload_images/16375643-b1cbd2208d23757e.png)

选择左侧导航  Personal access tokens (个人访问令牌), 点击 Generate new token (生成新的令牌)

![](http://upload-images.jianshu.io/upload_images/16375643-462748dfab788175.png)

设置 token 名字勾选 gist 点击创建 token

![](http://upload-images.jianshu.io/upload_images/16375643-aad23fcd209d9082.png)

复制 token 值（ 记住这个 token 值 ，此值只显示一次，之后要经常用到）

![](http://upload-images.jianshu.io/upload_images/16375643-72e08697965a53b7.png)
# 绘制漫天星