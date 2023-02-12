---
title: LeetCode-458 可怜的小猪
date: 2022-09-03 16:32:00
updated:
tags: [计算机, 算法, LeetCode, C++]
categories: LeetCode题解
cover: https://raw.githubusercontent.com/wsylab/wsyPicGo/main/img/photo-1508589938009-83a346342bd6.avif
description: LeetCode-458「可怜的小猪」的思考与解答
---
### [LeetCode-458 可怜的小猪](https://leetcode.cn/problems/poor-pigs/)

### Solution 1
一只小猪在 $m$ 轮实验下的状态共有 $m + 1$ 种: 第 $1$ 轮死, 第 $2$ 轮死, ..., 第 $m$ 轮死, 以及一直活着. 如果有 $k$ 只小猪, 那么在 $m$ 轮实验下的状态则共有 $(m + 1)^k$ 种. 我们对水桶进行编码, 如果不超过 $(m + 1)^k$ , 那么一定存在合法编码. 实际上用 $m + 1$ 进制即可, 每只小猪负责一位, 依次枚举即可.
代码如下:
```C++
class Solution {
public:
    int poorPigs(int buckets, int minutesToDie, int minutesToTest) {
        int states = minutesToTest / minutesToDie + 1;
        int pigs = ceil(log(buckets) / log(states) - 1e-5);
        return pigs;
    }
};
```