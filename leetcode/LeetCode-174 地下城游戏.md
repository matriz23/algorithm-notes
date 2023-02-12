---
title: LeetCode-174 地下城游戏 
date: 2022-11-13 21:30:00
updated:
tags: [计算机, 算法, LeetCode, C++]
categories: LeetCode题解
cover: 
description: LeetCode-174「地下城游戏」的思考与解答
---
### [LeetCode-174 地下城游戏](https://leetcode.cn/problems/dungeon-game/)

### Solution 1
根据向右向下的移动限制, 不难想到动态规划. 本题值得思考的地方在于动态规划的计算顺序. 如果从左上角向右下角进行递推, 很难同时确定状态和方案数. 从右下角向左上角进行递推, 则很容易进行. 
记 $dp[i][j]$ 代表到达 $(i, j)$ 位置时至少需要多少体力. 考虑从 $dp[i][j]$ 到 $dp[i][j + 1]$ 的路径, 根据约束, 我们可以得到
$$
\begin{cases}
dp[i][j] \geq 1\\
dp[i][j] + dungeon[i][j] \geq dp[i][j + 1]
\end{cases}
$$

对于从 $dp[i][j]$ 到 $dp[i + 1][j]$ 的路径同理.
故有
$$
dp[i][j] = min(max(1, dp[i + 1][j] - dungeon[i][j]), max(1, dp[i][j + 1] - dungeon[i][j]))
$$

也就是
$$
dp[i][j] = max(1, min(dp[i + 1][j], dp[i][j + 1]) - dungeon[i][j])
$$

代码如下:
```C++
class Solution {
public:
    int calculateMinimumHP(vector<vector<int>>& dungeon) {
        int m = dungeon.size();
        int n = dungeon[0].size();
        vector<vector<int>> dp(m, vector<int>(n, 0));
        dp[m - 1][n - 1] = max(1, 1 - dungeon[m - 1][n - 1]);
        for (int i = m - 2; i >= 0; i--) {
            dp[i][n - 1] = max(1, dp[i + 1][n - 1] - dungeon[i][n - 1]);
        }
        for (int j = n - 2; j >= 0; j--) {
            dp[m - 1][j] = max(1, dp[m - 1][j + 1] - dungeon[m - 1][j]);
        }
        for (int i = m - 2; i >= 0; i--) {
            for (int j = n - 2; j >= 0; j--) {
                dp[i][j] = max(1, min(dp[i + 1][j], dp[i][j + 1]) - dungeon[i][j]);
            }
        }
        return dp[0][0];
    }
};
```