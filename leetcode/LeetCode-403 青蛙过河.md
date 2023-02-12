---
title: LeetCode-403 青蛙过河
date: 2022-09-05 17:05:00
updated:
tags: [计算机, 算法, LeetCode, C++, 动态规划]
categories: LeetCode题解
cover: https://raw.githubusercontent.com/wsylab/wsyPicGo/main/img/photo-1592827001593-78cad6b2e870.avif
description: LeetCode-403「青蛙过河」的思考与解答
---
### [LeetCode-403 青蛙过河](https://leetcode.cn/problems/frog-jump/)
### Solution 1
青蛙的状态有两个, 在第 $i$ 个石头上, 最后一步跳了 $j$ 步. 我们用 $dp[i][j]$ 来记录是否能够到达这一状态. 假设 $dp[i][j]$ 可以从第 $k$ 块石头转移而来, 那么必须是 $dp[k][stones[i] - stones[j] - 1], dp[k][stones[i] - stones[j]], dp[k][stones[i] - stones[j] + 1]$ 之一. 线性递推即可.
个别可以优化的点见代码. 
代码如下:
```C++
class Solution {
public:
    bool canCross(vector<int>& stones) {
        int n = stones.size();
        vector<vector<int>> dp(n, vector<int>(n, false));
        dp[0][0] = true;
        for (int i = 1; i < n; i++) {
            if (stones[i] - stones[i - 1] > i) {
                return false;
            }
        }
        for (int i = 1; i < n; i++) {
            for (int j = i - 1; j >= 0; j--) {
                int k = stones[i] - stones[j];
                if (k > j + 1) {
                    break;
                }
                dp[i][k] = dp[j][k - 1] || dp[j][k] || dp[j][k + 1];
                if (i == n - 1 && dp[i][k]) {
                    return true;
                }
            }
        }
        return false;
    }
};
```