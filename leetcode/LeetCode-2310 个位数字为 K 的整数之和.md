---
title: LeetCode-2310 个位数字为 K 的整数之和
date: 2022-06-20 20:20:00
updated:
tags: [计算机, 算法, LeetCode, C++]
categories: LeetCode题解
cover: https://raw.githubusercontent.com/wsylab/wsyPicGo/main/img/photo-1464061884326-64f6ebd57f83
description: LeetCode-「个位数字为 K 的整数之和」的思考与解答
---
### [LeetCode-2310 个位数字为 K 的整数之和](https://leetcode.cn/problems/sum-of-numbers-with-units-digit-k/)

### Solution 1
题目要求把 $num$ 拆分成尽可能少的个位为 $k$ 的数之和, 假设可以拆分为 $n$ 个数, 我们考虑最直接的约束 $k × i \equiv num\ mod\ 10$ , 由于题意中多重集约束非常宽松(允许有重复值), 只要把 $num$ 与 $k × n$ 之差(很显然这是 $10$ 的倍数)加到集合中的某一个元素上即可, 这里有一个隐藏条件 $num >= k × n$ .
要求的 $num$ 为 $0$ 时最好单独处理.
代码如下:
```C++
class Solution {
public:
    int minimumNumbers(int num, int k) {
        if (num == 0) {
             return 0;
        }
        for (int i = 1; i <= 10; i++) {
            if (k * i % 10 == num % 10 && k * i <= num) {
                return i;
            }
        }
        return -1;
    }
};
```