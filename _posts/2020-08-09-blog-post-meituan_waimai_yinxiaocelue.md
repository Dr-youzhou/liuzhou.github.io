---
title: '2021秋招-美团-外卖营销策略组-面试总结'
date: 2020-08-14
permalink: /posts/2020/08/meituan_waimai/
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

## 美团外卖-营销策略组 面经

部门概况：团队主要做美团外卖的外部推荐，用户优惠券包的发放，交易额度提升等策略。部门分成算法和工程两部分，算法的话大概有 12 个人左右。
北斗计划的话需要最少 4 轮技术面试，否则就需要 5 轮或者更多，去年因为北斗的人进的比较多，所以今年可能会更加严格一点。投的视觉岗位说人招满了没有坑位了。。。

## 一面（2020 年 8 月 14 日）

- 自我介绍
- 讲一片论文，针对论文中的一些问题进行提问
- 实际业务场景中如果需要给 1000 万用户发 100 万张金额一样的优惠券，目标是需要让最终的交易总额最大，怎么做。
- 如果优惠券总额是 100 万，不同用户得到的钱可以不一样，优化目标不变，这个该怎么建模。
- 通常我们认为发的优惠券金额越大，该用户成交额也会越大，但是如果根据历史数据训练出来的模型输出的预测结果不是这样的，有什么办法进行优化或者怎么解决这个问题。
- 你通常在遇到一个新的领域的话怎么去学习？
- 你通常在设计这种模型的时候，各种 layer，激活函数的这种设置是怎么样的考虑，或者参考什么
- 为什么选择 CV+NLP 这个方向。

- 算法题

```python
'''
如何高效计算一个完全二叉树的节点个数
'''

def func(root):
    if root is None:
        return 0
    l=get_deepth(root, flag=1)
    r=get_deepth(root, flag=-1)
    if l==r:
        return 2**(l)-1
    else:
        l=func(root.left)
        r=func(root.right)
        return l+r+1

def get_deepth(root,flag):
    if root is None:
        return 0
    cur_deepth=0
    if flag==1:
        cur_deepth= 1+get_deepth(root.left, flag)
    elif flag==-1:
        cur_deepth= 1+get_deepth(root.right, flag)
    return cur_deepth
```

<div data-hk-top-pages="5"> </div>
