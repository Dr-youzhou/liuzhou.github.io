---
title: '2020计算机视觉秋招提前批面试总结2'
date: 2020-06-22
permalink: /posts/2020/06/bytedance_ad_interview/
categories:
  - Interview
tags:
  - Interview,bytedance
toc: true
---

## 字节跳动，国际化广告变现部门

- 部门概况：主要是 TicTok 的全球广告算法模型相关，有一些上游的团队负责特征提取等工作。团队规模大概 18 人。

### 一面

- 自我介绍
- 对简历的一个项目的细节的问答

- 算法题 1

```python
'''
删除字符串中多余的空格
input = "   I am a bytedancer    "
output  = "I am a bytedancer"
'''
def func(s):
    if len(s)==0:
        return ''

    i=0
    while i<len(s) and s[i]==' ':
        i+=1
    if i==len(s):
        return ''
    s=s[i:]
    j=len(s)-1
    while j>=0 and s[j]==' ':
        j-=1
    s=s[:j+1]

    cache=[]
    for c in s:
        if c !=' ':
            cache.append(c)
        else:
            if cache[-1]!=' ':
                cache.append(c)
    result=''.join(cache)

    return result


if __name__=="__main__":
    input_str=input()
    result=func(input_str)
    print(result)
```

- 算法题 2
  - 实现用的 DFS，主要思路就是每次删除一个字符，直到长度和 target 相同时进行比较。不过如果输入的字符串过长的话，这个的时间复杂度会很高，这样的话按照每次选择一个字符来增加字符长度的思路应该效率会更高$C^6_{N}$。

```python
'''
好多牛牛
限定语言：C、Python、C++、Javascript、Python 3、Java、Go
给出一个字符串S，牛牛想知道这个字符串有多少个子序列等于"niuniu"
子序列可以通过在原串上删除任意个字符(包括0个字符和全部字符)得到。
为了防止答案过大，答案对1e9+7取模
示例1
输入
"niuniniu"
输出
3
说明
删除第4，5个字符可以得到"niuniu"
删除第5，6个字符可以得到"niuniu"
删除第6，7个字符可以得到"niuniu"
'''
result=0
def solve( s ):
    if len(s)<6:
        return 0
    if len(s)==6:
        return 1 if s=='niuniu' else 0
    func(s,0)

def func(s,begin_idx):
    global result
    if len(s)<=6:
        if s=='niuniu' :
            result+=1
        return
    for i in range(begin_idx,len(s)):
        sub_str=s[:i]+s[i+1:]
        #print(i,sub_str)
        func(sub_str,i)
    return

input_str=input()
solve(input_str)
print(result)

```

- 问面试官关于部门和业务相关的问题。

## 二面

- 医疗 AI 领域的数据缺乏问题怎么解决？

  - 数据增强，Loss 加权重，找医院合作标数据

- 虚函数了解吗？

  - 用于实现多态,[详解](https://www.zhihu.com/question/23971699?sort=created)

- 广告线上系统如果出现了过高预估的问题怎么解决？

  - 不太懂，瞎答的。针对高估的部分数据训练一个分类器，新来的数据先判断是否容易被高估，如果是容易被高估的数据，再额外训练一个纠偏的模型预测偏移量。来了新的 sample 先判断是否是容易高估的数据，如果不是直接用原始系统输出，如果是容易高估的数据，线上系统的结果减去预估偏移量得到的结果作为最终结果。
  - 有点打补丁或者 Boosting 算法意思。

- self-attention 的机制解释一下，为啥叫 self-attention

  - self-attention 最开始是在 NLP 领域提出的，self 主要是针对于句子内部的单词相互之间计算 attention 信息。而相对的不同句子之间的 attention 通常叫做 cross-attention。

- 视觉的话 segmentation 的这问题是怎么建模的？

  - 用 CNN 作为特征提取器，在 feature map 上做 pixel-wise 的分类问题。

- 是否了解过传统的机器学习算法

  - 了解过一些决策树，SVM 相关的算法，但是平时不怎么用。

- 视觉中会要使用传统的那些特征嘛？
  - 目前来说，基本都是深度学习一把梭，直接用 NN 提取特征进行端到端训练。

* 算法题 1

```python
'''
给定一个二叉树，原地将它展开为链表。
例如，给定二叉树

    1
   / \
  2   5
 / \   \
3   4   6
将其展开为：
1
 \
  2
   \
    3
     \
      4
       \
        5
         \
          6
'''
#coding=utf-8
import sys
#str = input()
#print(str)
class TreeNode:
    def __init__(val):
        self.val=val
        self.left=None
        self.right=None

stack=[]
def func(root):
    global stack
    if root is None:
        return None
    if root.right is not None:
        stack.append(root.right)

    if root.left is not None:
        nxt=root.left
    elif len(stack)>0：
        nxt=stack.pop(-1)
    else:
        nxt=None
    root.right=nxt
    self.func(nxt)
    return root

if __name__=="__main__":
    input_node=input()
    result=func(input_node)
    return result
```

- 算法题 2

```python
'''
给定一个由正整数组成且不存在重复数字的数组，找出和为给定目标正整数的组合的个数。
示例:
nums = [1, 2, 3]
target = 4
所有可能的组合为：
(1, 1, 1, 1)
(1, 1, 2)
(1, 2, 1)
(1, 3)
(2, 1, 1)
(2, 2)
(3, 1)
'''
#coding=utf-8
result=0
target=0
nums=[]
def func(pre_sum):
    global nums,result,target
    if pre_sum==target:
        result+=1
        return
    if pre_sum>target:
        return
    for i in range(len(nums)):
        cur_sum=pre_sum+nums[i]
        func(cur_sum)
    return

if __name__=='__main__':
    global nums,target,result
    input_data=input()
    tmp=input_data.split(',')
    tmp=[int(x) for x in tmp]
    target=int(input())
    tmp.sort()
    tmp=[x for x in tmp if x<=target]
    func(0)
    print(result)

```
