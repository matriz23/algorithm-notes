---
title: LeetCode-2304 网格中的最小路径代价 
date: 2022-06-22 13:19:00
updated:
tags: [计算机, 算法, LeetCode, C++, 动态规划]
categories: LeetCode题解
cover: https://raw.githubusercontent.com/wsylab/wsyPicGo/main/img/cactus.jpg
description: LeetCode-2304「网格中的最小路径代价」的思考与解答
---
### [LeetCode-2304 网格中的最小路径代价](https://leetcode.cn/problems/minimum-path-cost-in-a-grid/)

### Solution 1
本题和经典问题[LeetCode-931 下降最小路径和](https://leetcode.cn/problems/minimum-falling-path-sum/)很相似, 不同之处在于多出了节点路径的代价, 这一点不影响动态规划的思路. 记 $dp[i][j]$ 为 $(i, j)$ 出发到达最后一行的最小代价, 则有如下状态转移方程: 
$$
dp[i][j] = \underset{0\leq k\leq n - 1}{min}(dp[i + 1][j] + grid[i][j] + moveCost[grid[i][j]][k])
$$
最后 $\underset{0\leq j\leq n - 1}{min}dp[0][j]$ 即为所求最小代价.
代码如下:
```C++
class Solution {
public:
    int minPathCost(vector<vector<int>>& grid, vector<vector<int>>& moveCost) {
        int m = grid.size();
        int n = grid[0].size();
        vector<vector<int>> dp(m, vector<int>(n, 1000000007));
        for (int j = 0; j < n; j++) {
            dp[m - 1][j] = grid[m - 1][j];
        }
        for (int i = m - 2; i >= 0; i--) {
            for (int j = 0; j < n; j++) {
                for (int k = 0; k < n; k++) {
                    dp[i][j] = min(dp[i][j], dp[i + 1][k] + grid[i][j] + moveCost[grid[i][j]][k]);
                }
            }
        }
        int ans = dp[0][0];
        for (int j = 1; j < n; j++) {
            ans = min(ans, dp[0][j]);
        }
        return ans;
    }
};
```