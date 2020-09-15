---
title: '2021秋招-依图科技-面试总结'
date: 2020-09-15
permalink: /posts/2020/09/yitu/
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

## 2021 秋招 依图科技 面经

部门概况：面试的部门主要是做安防的，包括 human pose，人脸识别，身份识别等。

## 一面（2020 年 9 月 02 日）

- 自我介绍
- 简单介绍了一下 CVPR 的论文
- 写代码实现 BN 的 forward 过程

```python
import torch

def BN_forward(x):
    B,C,H,W=x.shape
    mean_val=x.mean(dim=0,keepdim=True).mean(dim=2,keepdim=True).mean(dim=3,keepdim=True)
    var_val=((x-mean_val)**2).mean(dim=0,keepdim=True).mean(dim=2,keepdim=True).mean(dim=3,keepdim=True)
    normed_x=(x-mean_val)/(var_val**0.5)
    gamma=1
    bais=0
    result=gamma*normed_x + bais
    return result

if __name__=='__main__':
    x=torch.randn(10,3,5,5)
    bn = nn.BatchNorm2d(num_features=3, eps=0, affine=False, track_running_stats=False)
    official_result=bn(x)
    my_result=BN_forward(x)
    diff=(official_result-my_result).sum()
    print(diff)

```

- Layer Normalization, Group Normalization 等的区别？ -[Link](https://zhuanlan.zhihu.com/p/91965772)

- 实现一下 resnet 的 basic block

```python
class BasicBlock(nn.Module):
    expansion = 1

    def __init__(self, inplanes, planes, stride=1, downsample=None):
        super(BasicBlock, self).__init__()
        self.conv1 = conv3x3(inplanes, planes, stride)
        self.bn1 = nn.BatchNorm2d(planes)
        self.relu = nn.ReLU(inplace=True)
        self.conv2 = conv3x3(planes, planes)
        self.bn2 = nn.BatchNorm2d(planes)
        self.downsample = downsample
        self.stride = stride

    def forward(self, x):
        residual = x

        out = self.conv1(x)
        out = self.bn1(out)
        out = self.relu(out)

        out = self.conv2(out)
        out = self.bn2(out)

        if self.downsample is not None:
            residual = self.downsample(x)

        out += residual
        out = self.relu(out)

        return out
```

- 算法题实现 topk

```python
def topk(nums,k):
    if len(nums)<=k or len(nums)<=1:
        return nums
    cur=nums[0]
    left=0
    right=len(nums)-1
    while left<right:
        while left<right and nums[right]>=cur:
            right-=1
        nums[left]=nums[right]
        while left<right and nums[left]<=cur:
            left+=1
        nums[right]=nums[left]
    nums[left]=cur
    if len(nums)-left==k:
        return nums[left:]
    elif len(nums)-left>k:
        return topk(nums[left+1:],k)
    else:
        return nums[left:] + topk(nums[:left], k-(len(nums)-left))

if __name__=='__main__':
    nums=[1,3,2,4,5]
    k=3
    result=topk(nums,k)
    print(result)
```

<div data-hk-top-pages="5"> </div>
