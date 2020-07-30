---
title: '2021秋招提前批-快手多媒体内容理解部门-面试总结'
date: 2020-07-30
permalink: /posts/2020/07/kuaishou_mmu/
categories:
  - Interview
tags:
  - Interview
toc: true
---

---

---

<div>
<div class="button01">
      <visited_a href="#" display:inline>你是第<span data-hk-page="current"> - </span>位访客~</visited_a>
      <visited_p class="top">٩(๑^o^๑)۶</visited_p>
      <visited_p class="bottom">Σ(っ °Д °;)っ被你发现了！</visited_p>
</div>
<img align="center" width="100" src="{{ site.url }}/images/static/take_me.gif" alt="" display:inline>
</div>
---

## 快手 多媒体内容理解部门 面经

部门概况： 部门主要是做多媒体的内容理解，包括图像，文本，语音，搜索推荐等多个业务方向。大部门总体的人员大概有100多人。


## 一面 （2020 年 7 月 30 日）
面试主要是三个部分：项目相关，基础机器学习的知识，算法题

- 自我介绍，主要是介绍了一下自己做的论文，聊了一下当前实习所做的事情。
- 传统机器学习的问题，你对传统机器学习有哪些了解
- 逻辑回归和XGBoost有什么异同？
  - 都是做分类的模型，逻辑回归的话是线性模型，通过对特征的线性组合然后通过sigmoid激活函数来预测一个0-1之间的概率值
  - XGboost是树模型，通过Boosting策略不断提升。

- 逻辑回归的Loss函数
  - 交叉熵损失函数

- 交叉熵函数和极大似然估计的关系
  - 交叉熵对应着最小化KL散度，最小化KL散度等价于最大似然估计
  - [详解](https://zhuanlan.zhihu.com/p/37917476)

- L1和L2的regression有什么作用
  - L2的正则化导致参数都是较小的值，能够一定程度上起到抑制过拟合的效果
  - L1的正则化能够起到一定的参数选择的作用，会导致稀疏解

- L1和L2的正则化对先验数据的分布假设分别是什么样子？
  - L1是拉普拉斯先验，而L2是高斯先验。
  - [详解](https://www.cnblogs.com/USTC-ZCC/p/10123610.html)

- 算法题一
```python
'''
给你一个整数数组 nums ，请你找出数组中乘积最大的连续子数组（该子数组中至少包含一个数字），并返回该子数组所对应的乘积。
nums = [1, 3, 5, -5, 6, -9, 20, 17]
'''
def func(nums):
    dp=[[0]*2 for i in range(len(nums))]
    result=nums[0]
    pre_max=nums[0]
    pre_min=nums[0]
    for i in range(1,len(nums)):
        cur=nums[i]
        if cur>=0:
            pre_max=max(pre_max*cur,cur)
            pre_min=min(pre_min*cur,cur)
        else:
            pre_max=max(pre_min*cur,cur)
            pre_min=min(pre_max*cur,cur)
        result=max([result,pre_min,pre_max])
    return result
 
if  __name__=='__main__':
    nums=[int(x) for x in input().split(',')]
    result=func(nums)
    print(result)

```

- 算法题2
```python
'''
给你一个整数数组 nums ，请你找出数组中的位置k，使得数组前k个元素的方差和后n-k个元素的方差之和最小。
'''

def func(nums):
    n=len(nums)
    total_sq_sum=sum([x**2 for x in nums])
    total_sum=sum(nums)
    
    result=100000000000000
    pre_sq_sum=0
    pre_sum=0
    for i in range(n):
        pre_sum+=nums[i]
        pre_sq_sum+=nums[i]**2
        left=pre_sq_sum/(i+1) - (pre_sum/(i+1))**2
        
        tail_sq_sum=total_sq_sum-pre_sq_sum
        tail_sum=total_sum-pre_sum
        right=tail_sq_sum/(n-i-1) - (tail_sum/(n-i-1))**2
        result=min(result,left+right)
    return result
```



## 二面 （2020 年 7 月 30 日）
二面主要是开放性的系统设计题，加上自己一片论文的介绍。

- 你对搜索引擎有没有什么了解？
- 如果叫你设计一个视频搜索的系统怎么设计，用query文本去搜索短视频
- 目前的系统主要还是基于传统的搜索引擎的策略，基于文本和tag进行搜索和排序。但是一些用户会弄一些虚假的tag或者视频描述来恶意使自己的视频搜索排名较高，如何通过视觉的特征解决这个问题。
- 针对一些口语化的query如何进行建模
- 如果叫你自己去构建一个跨模态的语义匹配的数据集，你可以怎么构建，你可以去爬取任何你想要的数据
  - 可以使用rephrase的策略对文本数据进行扩充，可以使用image retrieval的策略进行图片数据的扩充。
  - 面试官说 百度他们会用query文本使用传统的搜索引擎技术返回的top视频作为和query文本匹配的数据进行训练。我感觉有点类似于AQE
- 如果是在搜索的时候，通常会包含粗排和精排的过程，那么在粗排的时候对query进行encoding是使用轻模型好还是重模型好？
  - 单纯的对query进行encoding的话可以使用重模型，在使用encoding得到的embedding进行大规模数据库相似度计算和排序的时候应该使用轻模型。

---

<div data-hk-top-pages="5"> </div>
