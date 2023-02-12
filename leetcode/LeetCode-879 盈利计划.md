---
title: LeetCode-879 盈利计划 
date: 2022-09-13 18:28:00
updated:
tags: [计算机, 算法, LeetCode, C++, 动态规划]
categories: LeetCode题解
cover: https://raw.githubusercontent.com/wsylab/wsyPicGo/main/img/photo-1651099799036-0da7f1ad685e.avif
description: LeetCode-879「盈利计划」的思考与解答
---
### [LeetCode-879 盈利计划](https://leetcode.cn/problems/profitable-schemes/)

### Solution 1
枚举所有可能的情况. 记 $dp[i][j][k]$ 为有 $i$ 个员工, 可选任务从 $j$ 开始, 还需盈利 $k$ 的方案数. 状态转移方程如下:
$$
dp[i][j][k] = 
\begin{cases}
dp[i][j + 1][k], \ if\ group[j] > i\\
dp[i - group[j]][j + 1][k - profit[j]],\ otherwise
\end{cases}
$$
我们可以用记忆化搜索来替代递推过程.
代码如下:
```C++
#define ll long long
class Solution {
public:
    const int MOD = 1e9 + 7;
    int m;
    int dp[110][110][110];
    vector<int> _group, _profit;
    int f(int n, int i, int p) {
        if (i == m && p <= 0) {
            return 1;
        }
        if (i == m && p > 0) {
            return 0;
        }
        if (dp[n][i][p] != -1) {
            return dp[n][i][p];
        }
        dp[n][i][p] = 0;
        if (n - _group[i] >= 0) {
            int np = (p >= _profit[i])? p - _profit[i]: 0;
            dp[n][i][p] = f(n - _group[i], i + 1, np);
        }
        dp[n][i][p] = (dp[n][i][p] + f(n, i + 1, p)) % MOD;
        return dp[n][i][p];
    }

    int profitableSchemes(int n, int minProfit, vector<int>& group, vector<int>& profit) {
        for (int i = 0; i < 110; i++) {
            for (int j = 0; j < 110; j++) {
                for (int k = 0; k < 110; k++) {
                    dp[i][j][k] = -1;
                }
            }
        }
        m = group.size();
        _group = group;
        _profit = profit;
        return f(n, 0, minProfit);
    }
};
```