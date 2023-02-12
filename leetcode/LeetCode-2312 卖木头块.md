---
title: LeetCode-2312 卖木头块 
date: 2022-06-22 12:48:00
updated:
tags: [计算机, 算法, LeetCode, C++, 动态规划]
categories: LeetCode题解
cover: https://raw.githubusercontent.com/wsylab/wsyPicGo/main/img/photo-1597113366853-fea190b6cd82
description: LeetCode-2312「卖木头块」的思考与解答
---
### [LeetCode-2312 卖木头块](https://leetcode.cn/problems/selling-pieces-of-wood/)

### Solution 1
对一个大木块, 我们可以选择纵向切割, 也可以选择横向切割, 其价值为小木块之和, 具有典型的动态规划的特点.
我们记 $dp[i][j]$ 为 $m×n$ 木块的最大价值, 穷举切割方式, 可以得到 
$$
dp[i][j] = max(\underset{1\leq k\leq i - 1}{max}(dp[i - k][j] + dp[k][j]), \underset{1\leq k\leq j - 1}{max}(dp[i][j - k] + dp[i][k])),
$$
进一步注意到切割的对称性, 可以优化为
$$ 
dp[i][j] = max(\underset{1\leq k\leq \frac{i}{2}}{max}(dp[i - k][j] + dp[k][j]), \underset{1\leq k\leq \frac{j}{2}}{max}(dp[i][j - k] + dp[i][k])).
$$
最终 $dp[m][n]$ 即为我们想要的答案.
代码如下:
```C++
class Solution {
public:
    long long sellingWood(int m, int n, vector<vector<int>>& prices) {
        vector<vector<int>> dp(m, vector<int>(n, 0));
        for (auto &price: prices) {
            dp[price[0]][price[1]] = price[2];
        }
        for (int i = 1; i < m; i++) {
            for (int j = 1; j < n; j++) {
                for (int k = 1; k <= i / 2; k++) {
                    dp[i][j] = max(dp[i][j], dp[i - k][j] + dp[k][j]);
                }
                for (int k = 1; k <= j / 2; k++) {
                    dp[i][j] = max(dp[i][j], dp[i][j - k] + dp[i][k]);
                }
            }
        }
        return dp[m][n];
    }
};
```