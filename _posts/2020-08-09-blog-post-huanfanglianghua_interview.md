---
title: '2021秋招-幻方量化-深度学习-面试总结'
date: 2020-08-09
permalink: /posts/2020/08/xiaomi/
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

## 幻方量化-深度学习研究员/工程师 面经

部门概况：研发团队貌似只有杭州有。

## 笔试 （2020 年 8 月 09 日）

- 算法题 1

```python

'''
压缩字符串，如果一个字符出现的次数大于等于3次的话，将他表示成 数字+字符

例如：   aaabccccd**zzz   -> 3ab4cd883z
'''
# coding=utf-8
import sys
def encode(s):
    # 返回编码后的字符串
    final = []
    pre = '0'
    cnt = 0
    for c in s:
        if c != pre:
            if cnt < 3:
                for i in range(cnt):
                    final.append(pre)
            else:
                final.append(str(cnt))
                final.append(pre)
            pre = c
            cnt = 1
        else:
            cnt += 1
    if cnt >= 3:
        final.append(str(cnt))
        final.append(pre)
    else:
        for i in range(cnt):
            final.append(pre)
    result = ''.join(final)
    return result

for l in sys.stdin:
    print(encode(l.strip()))
```

- 算法题 2

```python

# coding: utf-8

'''
设计一个滑动窗口类，窗口的大小为10，每次输入是多个连续交易日的价格数据，返回滑动窗口内的最大回撤

最大回撤定义为  最大的a[i]-a[j]  其中i，j均在窗口内
例如当前窗口为： [1,2,3,4,10,3,0,6,7,7] 最大回撤为10 = 10-0

返回每个输入最后的滑动窗口的最大值。

输入示例：
1,2,13,12,1,3,0,4,10,3,0,6,7,7
3,-3,4,5,6,7,1

示例输出：
10
6
'''
import sys
class RollingWindow(object):
    def __init__(self):
        # 请完成此功能
        self.data = []

    def append(self, n):
        # 请完成此功能
        self.data.append(n)

    def get_max_drawdown(self):
        # 请完成此功能
        final_data = self.data[-10:]
        tail_min = final_data[-1]
        result = 0
        for i in range(len(final_data) - 2, -1, -1):
            cur = final_data[i] - tail_min
            result = max(result, cur)
        return result

if __name__ == '__main__':
    for line in sys.stdin:
        w = RollingWindow()
        for x in line.split(','):
            w.append(int(x))
        print(w.get_max_drawdown())

```

- 算法题 3

```python
'''
判断一个输入的9*9 的二维数组是否能够构成有效的数独的解。
'''
# coding=utf-8

import sys


def check_sudoku(grid):
    # grid是list of lists, 使用grid[0][0]访问其中数字
    # 请完成此功能
    row = len(grid)
    if row != 9:
        return False
    row_cache = {i: [] for i in range(9)}
    col_cache = {i: [] for i in range(9)}
    grid_cache = {i: [] for i in range(9)}

    for i in range(row):
        cur_row = len(grid[i])
        if cur_row != 9:
            return False

    for i in range(9):
        for j in range(9):
            cur = grid[i][j]
            row_cache[i].append(cur)
            col_cache[j].append(cur)
            a = i // 3
            b = j // 3
            gn = a * 3 + b
            grid_cache[gn].append(cur)

    for i, nums in row_cache.items():
        nums=set(nums)
        if len(nums)!=9:
            return False
        for i in range(1,10):
            if i not in nums:
                return False
    for i, nums in col_cache.items():
        nums = set(nums)
        if len(nums) != 9:
            return False
        for i in range(1, 10):
            if i not in nums:
                return False
    for i, nums in grid_cache.items():
        nums = set(nums)
        if len(nums) != 9:
            return False
        for i in range(1, 10):
            if i not in nums:
                return False
    return True

if __name__ == '__main__':
    grid = []
    for line in sys.stdin:
        line = line.strip()
        if not line:
            print(check_sudoku(grid))
            grid = []
            continue
        grid.append([int(x) for x in line.split(',')])
    print(check_sudoku(grid))
```

- 问答题 1:

  - 对于逻辑回归（logistics regression），给出赋予每个样本不同权重的 L-2 正则化逻辑回归的交叉熵损失函数（含 sigmoid 函数），以及参数更新过程。

- 问答题 2:

  - 给出 SVM 推导过程，包含拉格朗日优化式子，以及样本权重表达式，截距表达式等。
  - 通过计算说明 SVM 可以通过核函数快速进行非线性推广的基本前提，并说明高斯核和多项式核的最本质差别是什么。

- 问答题 3

  - 对于 EM 算法，给定样本{x1,x2...xm},样本间独立，引入隐含变量 z，使得 p(x,z)最大化，先给出 p(x,z)的最大似然估计，然后由该似然推导出 EM 算法的 E-step 和 M-step。

- 问答题 4:
  - 给出卷积神经网络 CNN 的误差传播公式，以及权值更新公式（提示，反向传播的过程中分全连接层，卷积层和池化层，参数更新只计算卷积层和全连接层）

<div data-hk-top-pages="5"> </div>
