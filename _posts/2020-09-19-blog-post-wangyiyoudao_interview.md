---
title: '2021秋招-网易有道-面试总结'
date: 2020-09-19
permalink: /posts/2020/09/wangyiyoudao/
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

## 网易有道 视觉算法工程师 面经

部门概况：部门主要是负责网易有道的 ORC 技术，手写和印刷体的识别，作业批改等业务。在北京大概几十人的视觉团队。有机会解决北京户口，工作节奏 10-6-5.

## 一面 （2020 年 9 月 19 日）

- 自我介绍
- 实习工作
- 项目介绍

- 算法题

```python
'''
二维的矩阵中保存着0 和 1 两种值，问最大的实心全1的正方形边长是多少？
'''
def func(matrix):
    n=len(matrix)
    m=len(matrix[0])

    dp=[[0]*m for i in range(n)]
    result=0
    for j in range(m):
        dp[0][j]=1 if matrix[0][j]==1 else 0
        result=max(result,dp[0][j])
    for i in range(1,n):
        dp[i][0]=1 if matrix[i][0]==1 else 0
        result=max(result,dp[i][0])

    for i  in range(1,n):
        for j in range(1,m):
            if matrix[i][j]==1:
                dp[i][j]=min([dp[i-1][j],dp[i][j-1],dp[i-1][j-1]])+1
                result=max(result,dp[i][j])
    return result

if __name__=='__main__':
    t=input()
    n,m=[int(x) for x in t.split()]
    matrix=[]
    for i in range(n):
        t=input()
        row=[int(x) for x in t.split()]
        matrix.append(row)
    result=func(matrix)
    print(result)
```

- 算法题 2

```python
'''
判断一个点是否在一个凸多边形内部


主要思路是通过该点向右做射线，如果和多边形有奇数个交点说明是在内部，如果是有偶数个交点说明是在外部。
'''


def is_in_segment(p,p1,p2): # 判断共线的三个点，p是否在p1,p2组成的线段内
    if p[0]>=min(p1[0],p2[0]) and p[0]<=max(p1[0],p2[0]) and p[1]>=min(p1[1],p2[1]) and p[1]<=max(p1[1],p2[1]):
        return True
    else:
        return False

def get_cross(k1,b1,k2,b2): # 计算两条直线的交点
    if k1==k2:
        if b1==b2:
            return '#','#'
        else:
            return None,None
    if k1=='#':
        x=b1
        y=k2*x + b2
    elif k2=='#':
        x=b2
        y=k1*x+b1
    else:
        x=(b2-b1)/(k1-k2)
        y=(k1*x)+b1
    return x,y


def func1(p,plist):
    a,b=p
    lines=[] # 构造直线参数
    for i in range(len(plist)):
        x1,y1=plist[i-1]
        x2,y2=plist[i]
        if x1==x2:
            k='#'
            b=x1
        else:
            k=(y1-y2)/(x1-x2)
            b=y1-k*(x1)
        lines.append((k,b))

    cnt=set() # 记录过点p向右做的射线 和多边形的交点个数，之所以用set是因为可能射线和多边形的交点在多边形的角上，这样可能会被统计两次。
    for i, (k,b) in enumerate(lines):
        tx,ty=get_cross(0, b, k, b)
        p1=plist[i-1]
        p2=plist[i]
        if tx is not None and ty is not None:
            if tx=='#' and ty=='#' and is_in(p,p1,p2):
                return True
            if tx<a:
                continue
            flag=is_in((tx,ty),p1,p2)
            if flag:
                set.add((tx,ty))
    return True if len(cnt)&1==1 else False

```

<div data-hk-top-pages="5"> </div>
