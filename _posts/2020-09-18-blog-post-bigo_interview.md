---
title: '2021秋招-Bigo-面试总结'
date: 2020-09-18
permalink: /posts/2020/09/bigo/
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

## Bigo 深度学习算法研究员 面经

部门概况：部门主要是负责 Bigo 产品的视觉相关技术。分为前端和后端两块，后端部分包含一些内容的风控，审核等。前端的部分包含类似抖音这种的各种玩法特效，具体技术上比如人脸检测、关键点识别、美妆特效等。团队规模北京、上海、新加坡加起来大概几十人。北京可能 20 人左右。

## 一面 （2020 年 9 月 16 日）

面试主要是三个部分：

- 算法题

```python
'''
第一问：二维的矩阵中保存着-1 和 1 两种值，-1表示不能走，给定一个起始位置（a，b）和一个目标位置(c,d)判断从 起始位置到目标位置的最短需要走多少步？

这个可以简单实用BFS解决。


第二问： 如果矩阵中保存的是-1 和 其他正整数，正整数表示从当前点走到邻居节点的cost，问从起始位置到目标位置的最小cost是多少。
'''
import heapq
import sys

def func(matrix,a,b,c,d):
    if matrix[a][b]==-1 or matrix[c][d]==-1:
        return -1
    if a==c and b==d:
        return 0
    n=len(matrix)
    m=len(matrix[0])

    dist=[[sys.maxsize]*m for i in range(n)]
    dist[a][b]=0
    min_heap=[(0,a,b)]
    directions=[(-1,0),(1,0),(0,-1),(0,1)]
    while len(min_heap)>0:
        cur_dist,x,y=heapq.heappop(min_heap)

        if cur_dist>=dist[x][y]:
            continue

        for dx,dy in directions:
            nx=x+dx
            ny=y+dy
            if nx>=0 and nx<n and ny>=0 and ny<m and matrix[nx][ny]!=-1:
                new_dist=cur_dist+matrix[x][y]
                if new_dist<dist[nx][ny]:
                    dist[nx][ny]=new_dist
                    heapq.heappush(min_heap,(new_dist,nx,ny))

    return -1 if dist[c][d]>=sys.maxsize else dist[c][d]
```

- 堆的构建的复杂度是多少，堆的插入的复杂度，过程是什么样子？
- C++中 STL实现的数据结构有哪些
- C++11中有什么新的特性
- 进程和线程的区别
- 进程之间如何通信
- python里如何实现多线程，编程实现的过程，运行结束后怎么操作（join等待所有子线程结束）
- python里多进程的通信怎么实现
- 归并排序和快速排序分别有啥优缺点，归并排序通常有哪些应用场景？
- 矩阵的秩是什么概念，怎么直观理解他
- SVD分解是啥，他有那些作用？
  - [Link](https://www.cnblogs.com/endlesscoding/p/10033527.html)
- 传统机器学习了解多少，怎么利用多个模型提升最终的性能（Bagging ， Boosting）
- 如何评价一个样本不平衡的分类模型的性能
  - AUC
- 为什么AUC score是合适的一个评价指标？
- 有哪些解决样本不平衡问题的策略？
- 讲了一下论文。
- 讲了一下阿里实习的工作。

<div data-hk-top-pages="5"> </div>
