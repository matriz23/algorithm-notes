---
title: LeetCode-790 多米诺和托米诺平铺 
date: 2022-11-13 16:52:00
updated:
tags: [计算机, 算法, LeetCode, C++, 动态规划]
categories: LeetCode题解
cover: 
description: LeetCode-790「多米诺和托米诺平铺」的思考与解答
---
### [LeetCode-790 多米诺和托米诺平铺](https://leetcode.cn/problems/domino-and-tromino-tiling/)

### Solution 1
考虑平铺的状态转移, 由于一个末尾铺满的状态可能由末尾未铺满的状态转化而来, 我们用 $dp[i][s]$ 来记录长度为 $i$ , 前 $i - 1$ 列都已铺满, 末尾状态为 $s$ 的方案数量, 由 $n = 3$ 的样例不难发现转化关系.
代码如下:
```C++
class Solution {
public:
    #define ll long long
    const int MOD = 1e9 + 7;
    int numTilings(int n) {
        vector<vector<ll>> dp(n + 1, vector<ll>(4, 0));
        dp[0][3] = 1;
        dp[1][0] = 1;
        dp[1][3] = 1;
        for (int i = 2; i <= n; i++) {
            dp[i][0] = dp[i - 1][3];
            dp[i][1] = (dp[i - 1][0] + dp[i - 1][2]) % MOD;
            dp[i][2] = (dp[i - 1][0] + dp[i - 1][1]) % MOD;
            dp[i][3] = (dp[i - 2][3] + dp[i - 1][1] + dp[i - 1][2] + dp[i - 1][3]) % MOD;
        } 
        return dp[n][3];
    }
};
```