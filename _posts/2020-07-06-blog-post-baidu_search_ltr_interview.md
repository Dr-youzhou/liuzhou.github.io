---
title: '2020秋招提前批-百度搜索架构部LTR部门-面试总结'
date: 2020-07-06
permalink: /posts/2020/07/baidu_search_ltr_interview/
categories:
  - Interview
tags:
  - Interview
toc: true
---

---

<div class="button01">
      <visited_a href="#" display:inline>你是第<span data-hk-page="current"> - </span>位访客~</visited_a>
      <visited_p class="top">٩(๑^o^๑)۶</visited_p>
      <visited_p class="bottom">Σ(っ °Д °;)っ被你发现了！</visited_p>
</div>

---

## 百度搜索架构部，LTR 部门面试总结

部门概况，搜索的推荐自然排序算法。LTR（learning to rank）。主要负责自然结果的排序，然后后续会有加入一些商业结果和其他结果变成最终展现出来的样子。
团队目前 15 人左右，目前也会用一些 NN 的方法进行 feature 提取然后排序。搜索和推荐不太一样，搜索的时候很多是有人工的 tag 标注的。

## 一面

- 自我介绍
- 介绍了两篇论文
- 你是什么专业的

- 对数理统计熟悉吗，说一下大数定理

  - 大数定理大概就是 当样本量足够大的时候 频率等于概率，具体公式不太记得。。。。
  - [大数定理](https://baike.baidu.com/item/%E5%A4%A7%E6%95%B0%E5%AE%9A%E5%BE%8B/410082?fromtitle=%E5%A4%A7%E6%95%B0%E5%AE%9A%E7%90%86&fromid=9679413&fr=aladdin)

- 中心极限定理记得吗？

  - 。。。不记得
  - [中心极限定理](https://baike.baidu.com/item/%E4%B8%AD%E5%BF%83%E6%9E%81%E9%99%90%E5%AE%9A%E7%90%86/829451?fr=aladdin)

- 讲一下贝叶斯分类的原理？

  - 。。。依稀记得朴素贝叶斯分类器是利用贝叶斯公式计算后验概率。
  - [贝叶斯分类器](https://www.jianshu.com/p/6728b311338c)

- 讲一下 logistic regression

  - 全连接层输出+sigmoid 激活函数 得到一个 0-1 之间的概率值

- 你这个是深度神经网络里的，讲解一下最原始的逻辑回归

  - 就是一个权重 w 对特征进行加权求和然后加上一个 bias，然后送到 sigmoid 函数里面预测出一个 0~1 之间的概率。
  - [逻辑回归](https://www.jianshu.com/p/6728b311338c)

- 写一下逻辑回归的 loss

  - 交叉熵 loss： $-(ylog(h(x))+ (1-y)log(1-h(x)))$

- 为什么是 log？
  - 没太理解这个问题啥意思

* 对排序了解吗，写一下快速排序的代码

```python
def quick_sort(nums):
    if len(nums)==0:
        return
    a=nums[0]
    l=0
    r=len(nums)-1
    while l<r:
        while l<r and nums[r]>=a:
            r-=1
        while l<r and nums[l]<=a:
            l+=1
        nums[l],nums[r]=nums[r],nums[l]
    nums[l]=a
    quick_sort(nums[:l])
    quick_sort(nums[l+1:])
```

- 给一个数组怎么找到其中较大的 k 个数

  - 使用一个小根堆，每个数依次放入堆里，当堆的元素大于 k 的时候弹出堆顶元素，对所有的数依次操作，堆中保存的结果即为较大的 k 个数

- 时间复杂度多少

  - Nlog(k)

- 还有其他方法实现吗

  - 排序一下取前 k 个。。。

- 能用之前快排的思路做吗
  - 好像可以，每次判断当前的 l 将数组划分为左右两个部分，长度分别为 l_cnt, r_cnt

```python
if k==r_cnt:
  直接返回右半部分所有数字

elif k>r_cnt:
  返回右半部分所有数字 + func(nums[:l])中较大的k-r_cnt个数

else:
  返回func(nums[l+1:])中较大的k个数
```

- 实现 sqrt 函数
  - 可以使用二分法或者牛顿法

```python
'''
二分法:
'''

def sqrt(x):
    l=0
    r=x
    pre_mid=x
    while True:
        mid=(l+r)/2.0
        if abs(mid-pre_mid)<10**(-6):
            break
        if mid**2==x:
            return mid
        if mid**2>x:
            r=mid
        else:
            l=mid
        pre_mid=mid
    return pre_mid


'''
牛顿法：
'''
def sqrt(num: int) -> bool:
    pre_x = num
    while True:
      cur_x=0.5*(pre_x+ num/pre_x)
      if abs(cur_x-pre_x)<10**(-6):
        break
      else:
        pre_x=cur_x
    return cur_x
```

---

<div data-hk-top-pages="5"> </div>
