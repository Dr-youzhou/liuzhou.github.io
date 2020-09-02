---
title: '2021秋招-Pony.AI-面试总结'
date: 2020-09-02
permalink: /posts/2020/09/pony-ai/
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

## 2021秋招 小马智行 面经

部门概况：根据背景面试官表示可能回去感知组。

## 一面（2020 年 9 月 02 日）

- 自我介绍
- 简单介绍了一下CVPR的论文

- 算法题

```python

'''
二维平面上有N个点，每个点用二维坐标表示，找到三个点让他们组成三角形，使得其他所有的点都不在三角形内部，返回这样的三个点的坐标。
'''

###### 想了个Nlog（N）的，先对所有的点按照x轴坐标排序，然后连续的三个点就有可能是满足条件的点。但是需要判断一下多个点是否共线这种情况。
def func(points): 
    points.sort(key=lambda x:x[0])
    for i in range(len(points)-2):
        j=i+1
        k=i+2
        xs=set([p[0] for p in points[i:i+3]])
        ys=[p[1] for p in points[i:i+3]]
        if len(xs)==1:
            continue
        elif len(xs)==2:
            return points[i:i+3]
        if len(set(ys))==1:
            continue
        else:
            return points[i:i+3]
    return []

# PS:面试官表示还有O(N)的做法，首先选三个不共线的点组成原始的三角形，然后依次扫描其他的点，如果他们是在三角形的外部则直接跳过，否则的话该点替换掉其中的一个点，最后所有的点都判断完毕之后得到的三角形上的三个点也满足题目要求。
```

<div data-hk-top-pages="5"> </div>
