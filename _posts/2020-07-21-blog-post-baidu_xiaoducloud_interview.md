---
title: '2021秋招提前批-百度小度云平台部-面试总结'
date: 2020-07-21
permalink: /posts/2020/07/baidu_xiaoducloud_interview/
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

## 百度 小度云平台部 视觉部门面试总结 

部门概况： 主要是做小度音响上的摄像头相关的算法，例如人脸相关的，儿童年龄检测，手势相关的，human pose 相关，以及一些安防相关的视觉算法。部门目前有 5 个人，3 个人负责算法，2 个人负责工程 sdk 的开发和封装等。算法一部分在端上，一部分在云上。

## 一面 （2020年7月21日）

- 自我介绍
- 介绍在腾讯实习的做的项目，技术实现等
- 介绍最近的一个项目，以及在项目中承担的角色，用到的算法等
- 简单介绍了一下发表的论文
- 视觉小样本的问题，怎么解决
  - 数据增强，flip，rotation，random crop，对比度饱和度等调整
  - 针对样本不平衡的问题，重采样，weighted loss 等
  - 外部数据做弱监督
- 缓解过拟合的方法

  - Dropout，BN，数据增强，参数正则化，验证集上的 early stop

- 算法题一

```python
''' n行m列的棋盘格，每次只能向下或者向右走一步，问从左上角到右下角有多少种走法。'''
def func(n, m): # 方法一，动态规划
    dp = [[0] * m for i in range(n)]
    dp[0][0] = 1
    for j in range(1, m):
        dp[0][j] = 1
    for i in range(1, n):
        dp[i][0] = 1

    for i in range(1, n):
        for j in range(1, m):
            dp[i][j] = dp[i - 1][j] + dp[i][j - 1]
    return dp[-1][-1]

def func_by_math(n,m): # 方法二，数学上的组合问题，总共需要走n+m-2步，其中有n-1步是向下走的，所以组合的种类就是从n+m-2中选择n-1，即C_{n+m-2}^{n-1}
    '''
    return C^{n-1}_{n+m-2}
    '''
    a=1
    b=1
    for i in range(n-1):
        a*=(n+m-2-i)
        b*=(i+1)
    return a//b

if __name__ == '__main__':
    n = int(input())
    m = int(input())
    result = func(n, m)
    print(result)

```

- 算法题二

```python
'''给定5个数，每次只能给其中的四个数+1，问最少操作多少次最终五个数会相同'''

'''每次给四个数加一相当于每次只给一个数减一，所以最后结果是所有数和最小的数的差的和'''

def func(nums):
    a=min(nums)
    cache=[n-a for n in nums]
    return sum(cache)
```

---

<div data-hk-top-pages="5"> </div>
