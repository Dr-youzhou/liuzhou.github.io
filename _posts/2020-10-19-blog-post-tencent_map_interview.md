---
title: '2021秋招-腾讯地图-面试总结'
date: 2020-10-19
permalink: /posts/2020/10/tencent_map/
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

## 腾讯地图 应用研究 面经

部门概况：北京腾讯地图部门，主要负责地图的检索以及信息流的推荐相关业务。部门大概30多个人，地点在北京总部。

## 一面 （2020 年 10 月 19 日）

- 自我介绍

- 讲解CVPR的论文。
 
- 讲解了一下AAAI的论文。

- 讲了一下在阿里的实习经历。

- 算法题 1

```python
'''
实现split函数
'''
def split(s):
    result=[]
    t=[]
    for c in s:
        if c==' ':
            if len(t)>0:
                result.append(''.join(t))
            t=[]
        else:
            t.append(c)
    if len(t)>0:
        result.append(''.join(t))
    return result
```

- 算法题 2

``` python
'''
链表反转
class ListNode:
    def __init__(self,val):
        self.val=val
        self.next=None
'''

def reverse_link_list(head):
    if head is None:
        return None
    pre=None
    cur=head
    nxt=cur.next
    while nxt is not None:
        cur.next=pre
        pre=cur
        cur=nxt
        nxt=cur.next
    cur.next=pre
    return
```

- 算法题3
``` python
'''
计算一个数组的逆序对数
'''

def reversePairs(nums,begin,end):
    if end<=begin:
        return 0

    mid=(begin+end)>>1
    l_cnt=func(nums,begin,mid)
    r_cnt=func(nums,mid+1,end)

    t=[]
    cur_cnt=0
    l=begin
    r=mid+1
    while l<=mid  and r<=end:
        if nums[r]<nums[l]:
            cur_cnt+= (mid-l+1)
            t.append(nums[r])
            r+=1
        else:
            t.append(nums[l])
            l+=1
    while l<=mid:
        t.append(nums[l])
        l+=1
    while r<=end:
        t.append(nums[r])
        r+=1

    for i in range(len(t)):
        nums[begin+i]=t[i]
    return l_cnt+r_cnt+cur_cnt
```


<div data-hk-top-pages="5"> </div>
