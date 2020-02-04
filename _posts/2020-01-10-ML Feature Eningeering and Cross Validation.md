---
layout:     post
title:      Machine Learning - Feature Engineering and Cross Validation
subtitle:   ML 笔记总结 02
date:       2020-01-10
author:     Kaiyun
header-img: img/post-bg-ios9-web.jpg
catalog: true
tags:
    - Machine Learning
---
ML02

 特征工程 
 将特征值的范围缩到统一范围 （通常是0-1）


 交叉验证

 数值型特征 如长度，宽度， 像素等

 - 可直接用 
 - 但是对于模型来说 数值范围归一化 （feature ormalization）
 提高模型的性能 如： 线性回归 kNN，SVM 神经网络等

 sklearn.preprocessing.MinMaxScaler()


 有序特征， 如等级 （A,B,C） 
 转化为有序数值 A->1 B->2 C->3

 类别型特征 如：性别（男，女）
 独热编码（One-Hot Encoding） 男-> 01 ; 女 ->10

 sklearn.preprocessing.OneHotEncoder()



 交叉验证

 参数调整 
 模型自身的参数 1.通过样本学习得到的参数。例如：逻辑回归以及神经网络中的权重及偏置的学习
 2. 超参数，模型框架的参数 如kmeans 中的k,神经网络中的网络层数及每层的节点个数 通常由手工设定


 调整参数的方法：
  1.交叉验证 
  折数 * 候选参数个数
  	sklearn.model_selection.cross_val_score()
  2.网络搜索 （Gird Search） 多个超参数
  sklearn.model_selection.GirdSearchCV()



 模型评价指标及模型的选择
建模过程： 样本表示： 提取并选择样本特征 -> 训练模型：在训练样本上训练模型 -> 评估模型 -

                     ->调整特征和模型 |> 


评估模型：
不同的应用有不同的目标， 不同的评价指标
准确率（accurancy）是最常见的一种
但是准确率越高越好吗？
假设，在1000 个样本中 有999 个样本 1个负样本（不均衡数据集）
如果全部预测正样本 就可以得到准确率 99.9% ！ 这样的场景有信用卡欺诈检测，离职员工检测等
有些任务更关心的是某一个类的准确率而不是整体的准确率


真正例（TP） 预测值是1，真实值是1. 被正确分类的正例样本
假正例（FP） 预测值是1， 但是真实值是0；
真反例（TN） 预测值是0， 真实值是0；
假反例 (FN)  预测值是0，但是真实值是1

TPR（recall, 召回率） TP/(TP + FN) 表示检测率
Precision （精确率） ： TP/(TP+FP) 
FRP : FP/(TN+FP),在所有实际值是0的样本中

Precision-Recall Curve (PR 曲线)
x轴 : recall , y轴： preciion （可交换）
右上角是"最理想"的点， precision =1.0 , recall =1.0 

Receiver Operation Characteristic Curve (ROC 曲线)
x 轴： FPR， y 轴：TPR
又上角是 "最理想的点"， FPR =0.0  TPR =1.0


AUC 的值就是ROC曲线下的面积
AUC 在0~1 之间
0.5<AUC<1 优于随机猜测， 这个分类器 （模型） 妥善设定阈值的话， 能有预测的价值
AUC =0.5 跟随机猜测一样 （例、；丢铜板）模型没有预测价值
AUC < 0.5 比随机猜测还差， 但只要总是反预测而行，就优于随机猜测

混淆矩阵 （confusion matrix）
可用于多分类模型的评价

集成学习
通过构建并结合多个学习器来完成学习任务
同质集成 "homogeneous" ：集成中包含同类型的学习器 -> "基学习器" （base learner）
异质集成 "heterogeneous" : 集成中包含不同种类的学习器 -> "组件学习器" （component learner）

集成策略
  简单平均法
  加权平均法 （考虑权重）
  绝对数投票法，某标记投票过半， 则预测为该标记
  相对数投票法， 预测为投票最多的标记 
  加权投票法

集成策略
  stacking
  pip install mlxtend
  conda install mlxtend

Ensemble Learning 
好的集成学习 个体学习器应 "好而不同" ：个体学习器要有一定的 "准确性"
并且还要有"多样性"

个体学习器间不存在强依赖关系，可同时生成的并行化方法 （Bagging）
个体学习器间存在依赖关系，串行生成的序列化方法(Boosting)

Boosting 
1. 从初始训练集里面训练一个基学习器
2.根据基学习器的表现，对训练样本分布进行调整， 使得之前基学习器做错的训练样本在之后得到更多的关注
3.基于调整后的样本分布训练下一个基学习器
4.直至基学习器数目达到预设值T，最终将得到的T 个基学习器进行加权节后

代表算法:
	Adaboost : "加性学习" （additive model） 基学习器的线型组合

Gradient Boosted(-ing) Decision Tree

* 传统的Boosting :从初始训练集训练出一个基学习器， 再根据基学习器的表现对训练样本分布进行调整 是的先前基学习器做错的训练样本在后续收到更多的关注， 然后基于调整后的样本分布来训练下一个基学习器；直到基学习器数目达到预设的T值
最终将这T个基学习器进行加权结合

* Gradient Boost 是一个框架， 可以套入不同的模型 与传统的Boosting的区别是， 每一次的计算是为了减少上一次的残差，而为了消除残差， 
可以在残差减少的梯度方向上建立新的模型

Gradient Boosted(ing) Decision Tree

构造一系列决策树 每棵树尝试去纠正之前的错误
每棵树学习的是之前所有书的结论和残差， 拟合得到一个当前的树，整个迭代过程生成的树的累加就是GBDT
学习率决定新的树去纠正错误的程度

高学习率： 生成复杂的树
低学习率：生成简单的树


残差：由学习上一棵树得到的 真实值和预测值之间的差值

GBDT关键参数：
n_estimator:包含的决策树（弱分类器）的个数
learning_rate: 学习率，控制从上一次迭代中纠错的强度
max_depth：通常不会设得过大， 比如在大多数应用中设置为3-5

优点：
在多数任务中都能取得不错的效果
不需要过多的使用特征归一化和参数调整
可以使用不同种类的特征

缺点：
结果不容易解释
需要注意对学习率的调参
训练会需要大量的计算
由于计算量的问题 不太适用于文本分类或者其他高维稀疏的特征

Bagging ， 基于自助采样法 （bootstrap sampling）
给定包含m个样本的数据集， 先随机去除一个样本放入采样集中 再把该样本放回初始数据集  使得下次采样时该样本仍可能被选中 这样经过m 次随机采样操作 得到包含m个样本的采样集，初始训练集中有的样本在采样集中多次出现，有的则未出现 （初始训练集中约有63.2%的样本出现在样本集中）
样本在m次采样中始终不被采集到的概率是（1-1/m）m次方,取极限 0.368
基于此，采样出T个含m个训练样本的采样集， 然后基于每个采样集训练出基学习器

随机森林在构建bagging 采样的基础上， 进一步在决策树的训练过程中引入随机属性选择， 对基决策树的每一个结点，先从该结点的属性集合（d个属性）中随机选择一个包含k个属性的子集，然后再从自己种选择最优属性进行划分

k=d,基决策树的构建与传统决策树相同
k=1,随机选择一个属性用于划分:通常k=log2 d

max_features 会影响随机森林的学习过程
max_features =1 构造出的是完全不同的森林， 其中的树较复杂
max_features = 接近特征的个数， 构造出相似的森林 ， 其中的树较简单

随机森林
关键参数：
1.n_estimators 包含决策树的个数
2.max_festure:对于模型影响很大，森林中决策树的差异性  通常选择sklearn 中默认的即可，个别情况需要人为调整
3.max_depth:每颗决策树的深度
4.random_state: 如果需要复现实验结果， 需要设置相同的random_state

优点：
应用广泛， 在多数任务中都能取得不错的效果
不需要过多的使用特征归一化和参数调整
可以使用不同类型的特征

缺点：
结果不容易解释
对于高维数据 可能没有线性模块快速和准确





 








