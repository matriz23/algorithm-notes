---
title: LeetCode-2267 检查是否有合法括号字符串路径 
date: 2022-05-08 20:50:00
updated:
tags: [计算机, 算法, LeetCode, C++, 动态规划, 路径搜索]
categories: LeetCode题解
cover: https://raw.githubusercontent.com/wsylab/wsyPicGo/main/img/photo-1512407864998-0aafd285362d
description: LeetCode-2267「检查是否有合法括号字符串路径」的思考与解答
---
### [LeetCode-2267 检查是否有合法括号字符串路径](https://leetcode.cn/problems/check-if-there-is-a-valid-parentheses-string-path/)
LeetCode周赛292T4, 考试时写了个深搜果然超时了...
深搜最好配合记忆化搜索, 转化成动态规划更好处理一点.
### Solution 1
首先注意到括号匹配问题相当于任何时候路径上的"("都不能少于")", 如何设计一个便于转移的状态? 从路径上的"("与")"差的个数不能为负数入手, 我们建立一个 $bool$ 数组 $dp[m][n][k]$ , 表示终点为 $i, j$ 且值为 $k$ 的路径是否存在. 考虑 $dp[m][n][k]$ 的值, 只需要观察这一格点的括号情况就可以结合已求出的 $dp$ 计算.
代码如下: 
```C++
class Solution {
public:
    bool hasValidPath(vector<vector<char>>& grid) {
        int m = grid.size();
        int n = grid[0].size();
        // 一个简单的剪枝
        if ((m + n) % 2 == 0) {
            return false;
        }
        // 不需要返回路径个数, 我们建立bool数组就好, 如果要路径个数记得开long long
        bool dp[110][110][1010];
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                for (int k = 0; k < m + n; k++) {
                    dp[i][j][k] = false;
                }
            }
        }
        if (grid[0][0] == '(') {
            dp[0][0][1] = true;
        }
        // 处理第0行
        for (int j = 1; j < n; j++) {
            for (int k = 0; k < m + n; k++) {
                if (grid[0][j] == '(' && k != 0) {
                    dp[0][j][k] = dp[0][j - 1][k - 1];
                }
                else if (grid[0][j] == ')'){
                    dp[0][j][k] = dp[0][j - 1][k + 1];
                }
            }
        }
        // 处理第0列
        for (int i = 1; i < m; i++) {
            for (int k = 0; k < m + n; k++) {
                if (grid[i][0] == '(' && k != 0) {
                    dp[i][0][k] = dp[i - 1][0][k - 1];
                }
                else if (grid[i][0] == ')') {
                    dp[i][0][k] = dp[i - 1][0][k + 1];
                }
            }
        }        
        // 处理其他的格子
        for (int i = 1; i < m; i++) {
            for (int j = 1; j < n; j++) {
                for (int k = 0; k < n; k++) {
                    if (grid[i][j] == '(' && k != 0) {
                        dp[i][j][k] = dp[i - 1][j][k - 1] + dp[i][j - 1][k - 1];
                    }
                    else if (grid[i][j] == ')'){
                        dp[i][j][k] = dp[i - 1][j][k + 1] + dp[i][j - 1][k + 1];
                    }
                }
            }
        }
        return dp[m - 1][n - 1][0];
    }
};
```