---
title: LeetCode-312 戳气球 
date: 2022-09-05 18:07:00
updated:
tags: [计算机, 算法, LeetCode, C++, 动态规划]
categories: LeetCode题解
cover: https://raw.githubusercontent.com/wsylab/wsyPicGo/main/img/photo-1611142288262-3bb8f5fc45d7.avif
description: LeetCode-312「戳气球」的思考与解答
---
### [LeetCode-312 戳气球](https://leetcode.cn/problems/burst-balloons/)

### Solution 1
把编号 $0, 1, ..., n - 1$ 的气球编号修改成 $1, 2, ..., n$ , 同时补上 $0$ 和 $n + 1$ 两个边界的虚拟气球 (值视为 $1$) . 对于每个开区间 $(i, j)$ , 记戳完区间内所有气球的最高得分为 $f(i, j)$, 考虑其最后一个被戳掉的气球, 假设为 $k$ , 则有 
$$
f(i, j) = \underset{i < k < j}{max}\{f(i, k) + f(k, j) + val[i]\times val[k]\times val[j]\}
$$ 
$f(0, n + 1)$ 就是我们要求的答案.
代码如下:
```C++
class Solution {
public:
    vector<vector<int>> dp;
    vector<int> a;
    int f(int i, int j) {
        if (dp[i][j] != -1) {
            return dp[i][j];
        }
        int res = 0;
        for (int k = i + 1; k < j; k++) {
            res = max(res, f(i, k) + f(k, j) + a[i] * a[k] * a[j]);
        }
        dp[i][j] = res;
        return res;
    }

    int maxCoins(vector<int>& nums) {
        int n = nums.size();
        a = vector<int>(n + 2, 1);
        for (int i = 1; i <= n; i++) {
            a[i] = nums[i - 1];
        }
        dp = vector<vector<int>>(n + 2, vector<int>(n + 2, -1));
        return f(0, n + 1);
    }
};
```