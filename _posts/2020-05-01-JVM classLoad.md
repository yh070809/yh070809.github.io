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

![类加载过程](https://github.com/yh070809/github.images/blob/master/images/classLoad.png)

### parents delegates 双亲委派机制 

When load a  class , first  delegates it's parents to find the target , if parents can not find then ask parents ' parents (upper lever) , if still can not find , then find the class under self path

这里类加载其实就有一个双亲委派机制，加载某个类时会先委托父加载器寻找目标类，找不
到再委托上层父加载器加载，如果所有父加载器在自己的加载类路径下都找不到目标类，则
在自己的类加载路径中查找并载入目标类。

advantage:
优点：

Safety： to protect core function class (e.g. String Class) not been override 

沙箱安全机制：自己写的String.class类不会被加载，这样便可以防止核心API库被随意篡改

prevent repeat load : if parents already load the class , then no need to reload the class again

避免类的重复加载：当父亲已经加载了该类时，就没有必要子ClassLoader再   加载一次

![双亲委派机制](https://ibb.co/zS0Gtpj)




 