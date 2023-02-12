---
title: LeetCode-329 矩阵中的最长递增路径
date: 2022-07-03 22:18:00
updated:
tags: [计算机, 算法, LeetCode, C++, 记忆化搜索, 动态规划]
categories: LeetCode题解
cover: https://raw.githubusercontent.com/wsylab/wsyPicGo/main/img/photo-1420768255295-e871cbf6eb81
description: LeetCode-329「矩阵中的最长递增路径」的思考与解答
---
### [LeetCode-329 矩阵中的最长递增路径](https://leetcode.cn/problems/longest-increasing-path-in-a-matrix/)

### Solution 1
这个经典问题还有一个别号叫做"滑雪问题". 记 $dp[i][j]$ 为 $(i, j)$ 为起点的最长递增路径长度. 显然我们需要从 $(i, j)$ 出发, 向四个方向搜索, 对于合法的后继节点 $(x, y)$ 应当有 $dp[i][j] = max\ dp[x][y] + 1$ . 我们利用深度优先搜索求出所有 $dp[i][j]$ 即可.
代码如下:
```C++
class Solution {
public:
    int dir[4][2] = {{1,0},{0,-1},{0,1},{-1,0}};
    int n, m, ans = 0;
    int dfs(int x, int y, vector<vector<int>> & grid, vector<vector<int>> & dp) {
        if (dp[x][y] != 0) {
            return dp[x][y];
        }
        int cnt = 1;
        for (auto& d: dir) {
            int tx = x + d[0];
            int ty = y + d[1];
            if (tx >= 0 && tx < m && ty >= 0 && ty < n && grid[tx][ty] > grid[x][y]) {
                cnt = max(cnt, dfs(tx, ty, grid, dp) + 1);
            }
        }
        dp[x][y] = cnt;
        return cnt;
    }
    
    int longestIncreasingPath(vector<vector<int>>& grid) {
        m = grid.size();
        n = grid[0].size();
        vector<vector<int>> dp(m, vector<int>(n, 0));
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                ans = max(ans, dfs(i, j, grid, dp));
            }
        }
        return ans;
    }
};
```

> [LeetCode-6110 网格图中递增路径的数目](https://leetcode.cn/problems/number-of-increasing-paths-in-a-grid/) 与本题类似.