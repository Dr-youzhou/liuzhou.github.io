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

## 一面 （2020 年 9 月 18 日）

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

## 二面 （2020年9月24日）

- 数学题： 一个很大的文件可能好几十个G，如何随机的取出其中的一行。
  - 蓄水池抽样问题。 对于第一行直接选择放在内存中，后面的第n行以1/n的概率选择保留下来替换内存中的那一行，所有的行处理完之后留在内存中的就是随机采样的结果。

- 数学题： 一个红绿灯只有红色和绿色两种状态，相互切换，一天中有N辆车路过该路口，记录下来他们的等红灯时间，推算红灯 和 绿灯分别的时间长度。
  - 我给出的答案是，首先选择N个记录中非零的值假设有 a个，0的值设有b个， 假设红灯的时间长度是x秒，那么如果N足够大的情况下等红灯的时间长度应该是在0-x之间的均匀分布。因此a辆车等红灯的时间长度的和 应该是（0+x)\*n/2 那么这个值应该是等于所有a个值的和， 所以有$\frac{(0+x)n}{2} =  \sum_{i=1}^a a_i$ 。通过这个公式可以求出x，那么绿灯时间长度应该是$\frac{x}{a} \* b$。
  - 但是面试官表示这样求出的x应该是小于$max(a_i)$的，但是实际上的红灯时间应该是大于等于$max(a_i)$的，好像也有道理。但是当N趋于无穷的时候这两个值应该是趋于相同。

- 算法题

``` python
'''
往一个完全二叉树中插入一个结点，保持其仍然是一个完全二叉树
'''

def insert_tree(root,val):
  if root is None:
    return TreeNode(val)
  if is_full_tree(root):
    p=root
    while p.left is not None:
      p=p.left
    p.left=TreeNode(val)
  else:
    if is_full_tree(root.left):
      insert_tree(root.right,val)
    else:
      insert_tree(root.left,val)
  return root

def is_full_tree(root):
  if root is None:
    return False
  l=get_depth(root,1)
  r=get_depth(root,-1)
  if l==r:
    return True
  else:
    return False


def get_depth(root,flag):
  if root is None:
    return 0
  if flag==1:
    return 1+get_depth(root.left,flag)
  elif flag==-1:
    return 1+ get_depth(root.right,flag)
```

- 上面的算法时间复杂度是多少
 - （log（N））^2

- 证明 $ (log(N))^2 < N $
  

<div data-hk-top-pages="5"> </div>
