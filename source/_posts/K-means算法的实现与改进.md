---
title: K-means算法的实现与改进
date: 2019-11-21 14:49:25
top: 10
tags:
- k-means
- python
categories:
- 机器学习
---

本文主要介绍了 K-means 算法的原理以及如何利用 python 去实现简单的 K-means 算法，然后对于 K-means 算法存在的一些问题进行了适当的改进。

<!-- more -->

## 聚类

### 聚类的定义

聚类就是对大量未标记的数据集，按照数据的内在相似性将数据集划分为多个类别，使同一类别内的数据相似性大而不同类别间的数据相似性小。聚类是典型的无监督学习。

### 聚类的一般方法

给定 N 个对象的数据集，将数据集划分为 k 个簇，k≤N，且满足：

1. 每个簇至少有一个对象

2. 每个对象只能属于一个簇

聚类既能作为一个单独过程，用于找寻数据的内在分布结构，也可以作为分类等其他任务的前驱过程。

### 不同类型的聚类

1. 原型聚类

	原型聚类也叫基于原型的聚类，此类算法假设聚类结构能够通过一组原型刻画，在现实聚类中极为常用。通常情况下，算法先对原型进行初始化，然后对原型进行迭代更新求解。采用不同的原型表示、不同的求解方式将产生不同的算法。其中比较典型的就是k均值算法，也叫 K-menas 算法。

2. 密度聚类

	密度聚类也叫基于密度的聚类，此类算法假设聚类结构能够通过样本的紧密程度确定。通常情况下，密度聚类算法从样本密度的角度来考察样本之间的可连接性，并基于可连接样本不断扩展聚类簇以获得最终的聚类结果。其中比较著名就是 DBSCAN 算法。

3. 层次聚类

	层次聚类试图在不同层次对数据集进行划分，从而形成树型的聚类结构。数据集的划分可以采用“自底向上”的聚合策略，也可以采用“自顶向下”的分拆策略。

4. 谱聚类

	谱聚类是一种基于图论的聚类方法，通过对样本数据的拉普拉斯矩阵的特征向量进行聚类，从而达到对样本数据聚类的目的。

## K-means 算法原理

首先随机选取 k 个对象，每个对象初始地代表了一个簇的平均值或中心，称为初始均值向量。对剩余的每个对象根据其与各个簇中心的距离，将它赋给最近的簇。然后重新计算每个簇的平均值得到新的均值向量。这个过程不断重复直到当前均值向量均保持不变。

算法描述：

```
从 D 中随机选择 k 个样本作为初始均值向量
repeat
    for j=1,2,…,m do
        计算样本与各均值向量的距离
        根据距离最近的均值向量确定样本的簇标记
        将样本划入相应的簇
    end for 
    for i=1,2,…,k do
        计算新的均值向量
        if 两个均值向量不相同  then
            更新均值向量
        else
            保持当前均值向量不变
        end if
    end for
until 当前均值向量均未更新
```

## K-means 算法实现

```py
from matplotlib import pyplot as plt
import random
import math
import copy
class Kmeans:
    __dataList = []
    __k = 1
    __kSample = []
    __count = 0
    def __init__(self,fileName,k):
        self.__dataList = self.__readData(fileName)
        self.__kSample = self.__getKSample(self.__dataList,k)
        self.__k = k

    # 开始聚类
    def start(self):
        while(True):
            #计算每一个点与均值向量之间的距离,确定每一个点的类别
            for index,item in enumerate(self.__dataList):
                self.__caculateType(self.__dataList[index],self.__kSample)
            # 保存均值向量的副本
            copiedKSample = copy.deepcopy(self.__kSample)
            # 重新计算均值向量
            self.__caculateKSampleByAverge(self.__kSample)
            # 如果两个均值向量相等，则循环停止
            if copiedKSample == self.__kSample:
                break
            self.__count += 1
    # 绘制散点图
    def drawPic(self):
        # 由于是二维坐标，因此只需x，y即可
        x = []
        y = []
        c = []
        for i in range(len(self.__dataList)):
            x.append(self.__dataList[i][0])
            y.append(self.__dataList[i][1])
            c.append(self.__dataList[i][2])
        plt.title("dataset k=" + str(self.__k))
        plt.scatter(x, y,c=c)
        plt.show()
    # 获取迭代次数
    def getCount(self):
        return self.__count

    # 从文件中读取数据
    def __readData(self,fileName):
        # 用存放数据的列表
        dataList = []
        try:
            fp = open(fileName,"r")
            fpList = fp.read().splitlines()
            # 将数据分割成二维列表
            for item in fpList:
                dataList.append(item.split("\t"))
            # 将字符数据转化成浮点数
            for i in range(len(dataList)):
                for j in range(len(dataList[i])):
                    dataList[i][j] = float(dataList[i][j])
            # 如果数据不包含类别信息
            # for i in range(len(dataList)):
            #     dataList[i].append(0)
        except IOError:
            print("error")
        #返回数据
        return dataList
    # 获取初始k个点，也就是初始均值向量
    def __getKSample(self,dataList, k):
        kSample = []    
        for i in range(k):
            #从所有数据集中随机选取k个数据
            num = random.randint(0,len(dataList)-1)
            kSample.append(copy.deepcopy(dataList[num]))
        return kSample
    
    # 计算两个点之间的距离
    def __getDistance(self,dataPoint1,dataPoint2):
            
        distance = 0
        # 因为每一项数据的最后一位为类别，所以不参与计算距离
        for i in range(len(dataPoint1)-1):
            distance = distance + pow(dataPoint1[i]-dataPoint2[i],2)		
        distance = math.sqrt(distance)
        return distance
    # 根据每个样本距离均值向量的长短，计算每个样本所属的类别
    def __caculateType(self,dataPoint,kSample):
        # 首先假设该样本距离第一个均值向量最近,即该样本属于第一类
        minDistance = self.__getDistance(dataPoint,kSample[0])
        # 记录该样本所属的类别
        type = 0
        
        # 计算该样本与每一个均值向量之间的距离
        for index,item in enumerate(kSample):
            distance = self.__getDistance(dataPoint,item)
            # 如果该数据点距离该类别较小
            if distance < minDistance:
                minDistance = distance  # 更新最短距离
                type = index    # 更新样本点所属类别
        # 修改数据点的类别
        dataPoint[len(dataPoint)-1] = type

    # 重新计算均值向量
    def __caculateKSampleByAverge(self,kSample):
         # 对于每个均值向量，其下标为类别
        for i in range(len(kSample)):
            typeI = []
            # 遍历所有数据找到与其类别一致的数据点
            for item in self.__dataList:
                if item[(len(item)-1)] == i:
                    typeI.append(copy.deepcopy(item))
            #求和
            for j in range(1,len(typeI)):
                for k in range(len(typeI[j])):
                    typeI[j][k] += typeI[j-1][k]
            # 求均值并更改每一类的聚类中心
            for j in range(len(kSample[i])):
                kSample[i][j] = typeI[len(typeI)-1][j]/len(typeI)

a = Kmeans("f://machine_learning/shape_sets/D31.txt",3)

a.start()
print(a.getCount())
#二维坐标才可以画散点图
a.drawPic()
```

## K-means 算法的改进

K-means 是随机选取的初始点，因此不同的初始点对聚类结果有较大的影响。为解决这一问题我们可采用概率的方式来选择初始点，即 K-means++。

基本思想：

1. 从输入的集合中随机选取一个点作为聚类的中心

2. 对于数据集中的每一个点，计算它与最近的聚类中心（指已选择的聚类中心）的距离 D(x)

3. 选择一个新的数据点作为新的聚类中心，选择的原则是：D(x) 较大的点，被选取作为聚类中心的概率较大

4. 重复2、3步骤直到 k 个初始聚类中心被选出

5. 利用选出的 k 个初始的聚类中心来运行标准的 K-means 算法

从上面可以看出，此算法的关键是如何将 D(x) 反映到点被选择的概率上，其方法如下：

1. 对于每个点都会计算与它最近的聚类中心的距离 D(x)，将每个点的 D(x)^2 保存在一个列表 List 中，然后把这些距离加起来，即对 List 中所有元素求和得到 SumList。

2. 对于 List 中的每个点计算 List[i]/SumList ，即每个点概率 P(x)

3. 将 P(x) 累加得到概率区间

4. 产生一个 0-1 的随机数，该数落在哪个区间内就选择哪个点作为新的聚类中心

具体示例如下：

8个点：(1,2),(1,2),(2,1),(2,2),(5,5),(5,6),(6,5),(6,6)

1. 随机选取一个点作为聚类中心，如3号点 (2,1)

2. 计算 D(x)^2,P(x)

{% asset_img 1.png %}

3. 产生 0-1 之间的随机数，如果该随机数落在 [0-0.007] 则选取1号点，[0.007-0.021] 则选取2号点，[0.021-0.028] 则选取3号点以此类推，很明显5，6，7，8号点被选取的概率较大，其距离3号点的距离较远。

基于以上方法，我们改写选择初始点的函数：

```py
def getKSample(dataList, k):
    kSample = []   
    # 首先随机选取一个数字为种子点 
    num = random.randint(0,len(dataList)-1)
    kSample.append(copy.deepcopy(dataList[num]))
    
    for i in range(k-1):
        # 用于保存距离的列表
        D = []
        # 用于存储概率的数组
        P = []
        # 对于每个点，我们都计算其和最近的一个“种子点”的距离D(x)^2并保存在一个数组里
        for item1 in dataList:
            minDistance = getDistance(item1,kSample[0])
            for item2 in kSample:
                distance = getDistance(item1,item2)
                if distance < minDistance:
                    minDistance = distance

            D.append(pow(minDistance,2))
        # 循环结束之后得到储存距离的数组
        sumD = 0
        # 然后把距离加起来
        for item in D:
            sumD = sumD + item
        # 计算每个样本被选为下一个聚类中心的概率
        sumP = 0
        for item in D:
            itemP = item/sumD
            sumP = sumP + itemP
            P.append(sumP)
        #随机产生0-1之间的数
        rand = random.random()
        # 计算该数落在哪个区间
        for index,item in enumerate(P):
            if rand < item:
                kSample.append(copy.deepcopy(dataList[index]))
                break       
    return kSample
```






