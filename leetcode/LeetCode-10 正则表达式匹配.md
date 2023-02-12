---
title: LeetCode-10 正则表达式匹配 
date: 2022-08-05 16:37:00
updated:
tags: [计算机, 算法, LeetCode, C++, 动态规划]
categories: LeetCode题解
cover: https://raw.githubusercontent.com/wsylab/wsyPicGo/main/img/marek-piwnicki-sOJKAxFDQi4-unsplash.jpg
description: LeetCode-10「正则表达式匹配」的思考与解答
---
### [LeetCode-10 正则表达式匹配](https://leetcode.cn/problems/regular-expression-matching/)

### Solution 1
用 $dp[i][j]$ 记录 $s$ 的前 $i$ 个字符和 $p$ 的前 $j$ 个字符是否匹配. 本题中存在两种匹配模式: 单字符匹配 ($s[i]$ 与 $p[j]$ 相同或 $p[j]$ 为单体通配符) 与多字符匹配 ($p[j]$ 为 $*$ , 可以匹配任意个 $p[j - 1]$) . 
如果 $p[j]$ 不为 $*$ , 则 $dp[i][j]$ 取决于 $dp[i - 1][j - 1]$ 与 $s[i]$ 和 $p[j]$ 的单字符匹配情况; 
如果 $p[j]$ 为 $*$ , 则根据 $s[i], s[i - 1], ...$ 与 $p[j - 1]$ 的匹配情况, $dp[i][j]$ 可以从 $dp[i - 1][j - 2], dp[i - 2][j - 2], ...$ 转化而来. 考虑这一匹配组合的特性, $.*$ (这里 $.$ 代表 $.$ 或者小写字母) 能够匹配一次字符后继续使用, 也可以不再匹配字符. 因此, 如果有 $s[i] == p[j - 1]$ , 则 $dp[i][j] = dp[i - 1][j] + dp[i][j - 2]$ , 即 $p$ 的前 $j$ 个字符如果可以匹配 $s$ 的前 $i$ 字符, 也一定能匹配前 $i - 1$ 个字符, 反之亦然, 因为前提条件保证了 $s[i] == p[j - 1]$ ; 而 $p$ 前 $j$ 个字符的末尾两个 ($.*$) 也可以不参与匹配, 即我们使用 $p$ 的前 $j - 2$ 个就可以完成匹配了. 这一部分状态转移简化运用了类似递归的思想.
初始情况下, $dp[0][0]$ 为 $true$ , 我们认为两个空字符串是可以匹配的.
代码如下:
```C++
class Solution {
public:
    bool isMatch(string s, string p) {
        int n = s.size();
        int m = p.size();
        auto match = [&](int i, int j) { //单字符匹配
            if (i == 0) {
                return false;
            }
            if (p[j - 1] == '.') {
                return true;
            }
            return s[i - 1] == p[j - 1];
        };
        vector<vector<bool>> dp(n + 1, vector<bool>(m + 1, false));
        dp[0][0] = true;
        for (int i = 0; i <= n; i++) {
            for (int j = 1; j <= m; j++) {
                if (p[j - 1] == '*') {
                    dp[i][j] = dp[i][j] + dp[i][j - 2];
                    if (match(i, j - 1)) {
                        dp[i][j] = dp[i][j] + dp[i - 1][j];
                    }
                }
                else {
                    if (match(i, j)) {
                        dp[i][j] = dp[i][j] + dp[i - 1][j - 1];
                    }
                }
            }
        }
        return dp[n][m];
    }
};
```