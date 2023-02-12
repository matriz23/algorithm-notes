---
title: LeetCode-44 通配符匹配 
date: 2022-08-30 16:06:00
updated:
tags: [计算机, 算法, LeetCode, C++, 字符串, 动态规划]
categories: LeetCode题解
cover: https://raw.githubusercontent.com/wsylab/wsyPicGo/main/img/photo-1509565840034-3c385bbe6451.jpg
description: LeetCode-44「通配符匹配」的思考与解答
---
### [LeetCode-44 通配符匹配](https://leetcode.cn/problems/wildcard-matching/)

### Solution 1
本题类似 [LeetCode-10 正则表达式匹配](https://leetcode.cn/problems/regular-expression-matching/) , 不过更简单一点. 用 $dp[i][j]$ 记录 $s$ 的前 $i$ 个字符和 $p$ 的前 $j$ 字符的匹配情况. 如果 $p[j - 1] \not = *$ , 那么 $dp[i][j] = dp[i - 1][j - 1] \& match(s[i - 1], p[j - 1])$ , 否则, $*$ 可以继续匹配, 即从 $dp[i - 1][j]$ 转化而来; 也可以不参与匹配, 从 $dp[i][j - 1]$ 转化而来.
代码如下:
```C++
class Solution {
public:
    bool isMatch(string s, string p) {
        int n = s.size();
        int m = p.size();
        vector<vector<bool>> dp(n + 1, vector<bool>(m + 1, false));
        dp[0][0] = true;
        auto match = [&](int i, int j) {
            if (i == 0) {
                return false;
            }
            if (p[j - 1] == '?') {
                return true;
            }
            return s[i - 1] == p[j - 1];
        };
        for (int i = 0; i <= n; i++) {
            for (int j = 1; j <= m; j++) {
                if (p[j - 1] == '*') {
                    dp[i][j] = (i == 0)? dp[i][j - 1]: dp[i - 1][j] + dp[i][j - 1];
                }
                else {
                    dp[i][j] = (i == 0)? match(i, j): dp[i - 1][j - 1] & match(i, j);
                }
            }
        }
        return dp[n][m];
    }
};
```