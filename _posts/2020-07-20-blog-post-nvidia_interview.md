---
title: '2021秋招-NVIDIA-AI工程师-面试总结'
date: 2020-07-20
permalink: /posts/2020/07/nvidia_interview/
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

## NVIDIA AI 工程师面试总结

部门概况：主要是负责 ToB 业务，帮助客户优化模型，提高模型小于训练速度，一些最新的模型的实现，可能会涉及到一些 CUDA 的编程，算子的实现等。部门分布在北京、上海、深圳。不加班 5 点下班，偶尔可能会需要和国外的团队合作，基本不需要早起和美国开会。面试官说顺利的话可能会有 4-6 轮的面试。

## 一面（2020 年 7 月 20 日）

一面主要面的是都是一些基础的问题，深度学习相关的 trick 和经验什么的。

- 深度学习训练的时候超参数，learning rate，参数初始化，batchsize 等如何设置，以及对模型最终收敛结果的影响，为什么？

  - batchsize 一般是按照显存大小尽可能大设置，然后将显存填满。batchsize 较大的话收敛会更快，但是也不是越大越好，大的 batchsize 更可能会收敛到一个不太好的局部最优解。batchsize 较小的话收敛会更慢，因为梯度的随机性更强，更有可能会产生震荡等情况，但是可能能够收敛到更好的 local optimal。
  - learning rate 主要跟 optimizer 有关，通常 sgd 初始 0.01，Adam 初始 1e-3，特定任务例如 finetune bert 等模型需要设置 1e-5。 在训练的过程中通常是随着训练的过程会逐步减小 lr，一开始较大是为了快速收敛，后面变小是为了在最优解附近防止震荡从而得到一个较好的结果。 例如训练 image net 的时候通常每 30 个 epoch 将 lr 减小 10 倍。 但是也有一些使用 lr scheduler 的操作，比如 training loss 多少个 epoch 下降小于阈值的时候就减小 lr。 也有一些任务会使用 warm up 或者 cosine lr 策略，主要想法是在陷入局部最优解的时候通过较大的 lr 跳出 local optimal 从而可以收敛到更好的效果。
  - 参数初始化主要是使用框架自带的默认的初始化，可能对于某些任务特定的初始化策略会有效，了解到有一些类似于 kaiming_normal,这种初始化策略但是了解的不多。

- 对 batch Normalization 的计算过程，训练和 inference 的时候的策略，优点缺点，为什么？

  - 计算过程：$y= \gamma \frac{x-E(x)}{\sqrt{var[x]}} + \beta$
  - 训练的时候 $E(x), var[x]$都是根据每个 batch 的 feature 及时计算的，$\gamma , \beta$根据梯度进行学习和调整
  - Inference 的时候$E(x) , var[x]$是全 epoch 的 feature 的均值和方差，$\gamma, \beta$是最后一次迭代后的值
  - BN 的优点：
  - 保证每一层的输入的分布偏差不会太大，能够一定程度增加训练的稳定性，加快训练速度和收敛速度。
  - 在使用 sigmoid 等激活函数的时候能够一定程度缓解和消除梯度消失的问题，同时能够加深网络深度。
  - 在一定程度上能够防止过拟合，BN 每次的 mini-batch 的数据都不一样，但是每次的 mini-batch 的数据都会对 moving mean 和 moving variance 产生作用，可以认为是引入了噪声，这就可以认为是进行了 data augmentation，而 data augmentation 被认为是防止过拟合的一种方法。因此，可以认为用 BN 可以防止过拟合。

- 介绍了一个最近做的项目，在其中担任的主要职责
- 模型的性能优化有哪些策略

  - 剪枝，量化，知识蒸馏的[介绍和差别](https://blog.csdn.net/Fannie_Peng/article/details/106397222)
  - 剪枝可以丢掉一些对结果影响较小的 kernel 等同于说减少一些 channel，fc 层的话可以丢掉一些不太重要的权重，然后对剪枝后的模型进行 fine-tune。具体的策略有哪些，不太清楚，貌似有用强化学习做这个的。

- NMS 如何进行计算优化，假设你有无穷的计算资源如何设计并行的线程进行计算。
  - 假设有 N 的 box，先对其按照 confidence score 进行排序分别编号 1-n
  - 然后可以使用 N^2 个线程计算两两 box 之家的 IOU 分数，计算的时候可以判断 j 是否大于 i，如果不是直接返回 0，否则话计算 IOU 并根据阈值进行判断是否抑制的，进而填充到一个 N\*N 的二维矩阵中，M[i][j]=0 表示 i 对 j 不抑制，M[i][j]=1 表示 i 对 j 进行抑制
  - 结果设置为一个一维的长度为 N 的向量初始化为全 0 即都保留，然后依次对矩阵中的每一行进行判断如果当前行对应的 box 在结果向量中已经值为 1 直接跳过，否则将当前行中为 1 的 box 编号在结果向量中也置为 1。
  - 最终的结果向量中 0 对应的 box 保留，1 对应的 box 抑制
  - 在第一步排序的过程中也可以使用多个线程先做局部的排序，然后再使用归并排序，在 N 较大的时候应该有提升。

---

<div data-hk-top-pages="5"> </div>
