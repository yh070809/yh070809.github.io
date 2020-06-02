---
layout:     post
title:      JVM classLoad
subtitle:   java notes
date:       2020-01-31
author:     Kaiyun
header-img: img/post-bg-ios9-web.jpg
catalog: true
tags:
    - Machine Learning
---

### classLoad Process 类加载过程

Java project will build in jar package, and run by "main" function. When Application runing , classLoad will load class to JVM 
多个java文件经过编译打包生成可运行jar包，最终由java命令运行某个主类的main函数启动程序，这里首先需要通过类加载器把主类加载到JVM。

java class not load once togerther , it's load when use 
主类在运行过程中如果使用到其它类，会逐步加载这些类。注意，jar包里的类不是一次性全部加载的，是使用到时才加载。

classLoad process has following step / 加载步骤 :

load >> verify >> prepare >> analysis >> init >> use >> unload
加载 >> 验证 >> 准备 >> 解析 >> 初始化 >> 使用 >> 卸载

![blockchain]('./img/classLoad.png' "classLoad Process")
