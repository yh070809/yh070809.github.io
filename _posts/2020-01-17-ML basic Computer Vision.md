---
layout:     post
title:      ML  Computer  Vision
subtitle:   ML 笔记总结 03
date:       2020-01-17
author:     Kaiyun
header-img: img/post-bg-ios9-web.jpg
catalog: true
tags:
    - Machine Learning
---
计算机视觉库OpenCV
图像数据基本概念及操作
常用的图像特征描述
K-Means聚类及图像压缩


定义：从图像和视频里提取数值或者符号信息的计算机系统
让计算机能够和人类一样“看到并理解” 图像

pip install opencv-contrib-python

RGB颜色空间
	-加法混色 彩色显示器
	-3通道
		- Red
		- Green
		- Blue
    一个像素颜色值
    	-（b,g,r）
    -取值范围
       [0,255]
       [0.0,1.0]


    RGB三通道彩色图
    	-图片 -> 3维矩阵([0,255])
    单通道灰度图
    	-亮度信息 （[0,255]）
    Gray = R*0.3 + G * 0.59 + B * 0.11


    openCV 的图像数据
    - 图像数据是由 NumPy的多维数组（narray）表示的
    -由openCV加载的图像数据可以调用其他常用的包进行处理和计算，如matplotlib，scipy等

    数据类型和像素值

    CV 中图像的像素值通常有以下两种处理范围
    1. 0-255, 0：黑色，255：白色
    2. 0-1, 0：黑色，1：白色

OpenCV 读取的图像像素范围是 0-255

OpenCV 图像IO
读取图像cv2.imread()
cv2.IMREAD_COLOR读取彩色图像数据
cv2.IMREAD_GRAYSCALE 读取灰度图图像

显示图像 cv2.imshow(),cv2.waitKey(0),cv2.destroyAllWindows()
保存图像 cv2.imwrite()

图像数据 
	 图像数据是多维数据
	 前两维表示了图像的高，宽 第三维表示图像的通道个数，比如RGB，第三个维度为3
	 因为有三个通道
	 灰度图没有四三个维度
分割和索引
	像操作ndarray一样操作即可
	彩色空间，RGB HSV， Gray

	RGB转Gray ，cv2.cvtColor(img ,cv2,COLOR_BGR2GRAY)
颜色直方图
	直方图是一种能快速描述图像整体像素值分布的统计信息
	skimage.exposure.histogram
	如：可以根据直方图选定阈值用于调节图像对比度

增强图像数据的对比度有利于特征提取，不论是从肉眼还是算法来看都有帮助
自动调整图像的对比度 cv2.equalizeHist()



滤波/卷积

在每个图片位置 （x,y）上进行基于邻域的函数计算

	- 滤波函数 -> 权重相加
		卷积核，卷积模板
		滤波器，滤波模板
		扫描窗

	不同功能需要定义不同函数
		图像增强
		-平滑/去燥
		-梯度/锐化

	信息提取，检测
		边缘，显著点，纹理
		模式



边界补充 （Padding）
  获得同尺寸输出的情况下
  卷积核越大，补充越大

补充类型
	补零 （zero-padding）
	边界修复 （replication）
	镜像 （reflection）
	块复制 （wraparound）

图像滤波
	滤波是处理图像数据的常用基本操作
	滤波操作可以去除图像中的噪声点，由此增强图像的特征

中值滤波
cv2.medianBlur（）
	奇数尺寸 
	3X3， 5X5， 7X7 ， 2n -1 X 2n -1

	操作原理 

	卷积域内的像素值从小到大排列
	取中间值作为卷积输出
    有效去除椒盐噪点 

 高斯滤波 
   奇数尺寸 
   3 X 3， 5 X 5 ， 7 X 7 ，2n -1 X 2n -1

 模拟人眼，关注中心区域
 有效去除高斯噪声
 人眼特征：离关注中心越远，感受精度越模糊

均值滤波 cv2.blur（）
	奇数尺寸
	3 X 3， 5 X 5 ， 7 X 7 ，2n -1 X 2n -1
	参数和为1

边缘检测




常用的图像特征描述
	颜色特征
	纹理特征
	形状特征
	opencv中的特征方法  
	https://docs.opencv.org/3.0-beta/doc/py_tutorials/py_feature2d/py_table_of_contents_feature2d/py_table_of_contents_feature2d.html






 









