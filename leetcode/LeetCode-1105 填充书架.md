---
title: LeetCode-1105 填充书架 
date: 2022-05-15 15:08:00
updated:
tags: [计算机, 算法, LeetCode, C++]
categories: LeetCode题解
cover: https://raw.githubusercontent.com/wsylab/wsyPicGo/main/img/photo-1515542706656-8e6ef17a1521
description: LeetCode-1105「填充书架」的思考与解答
---
### [LeetCode-1105 填充书架](https://leetcode.cn/problems/filling-bookcase-shelves/)

### Solution 1
动态规划问题.
记 $dp[i]$ 为前 $i$ 本书的最小书架高度, 思考状态转移方程. 对于新加入的一本书 $i$, 假设有 $i - j$ 本书和 $i$ 在同一层, 则这一层高度为 $max(books[k][1]), i - j\leq k \leq i$ , 同时其余层数高度为 $dp[j]$. 对于每个 $i \geq 1$ 我们遍历 $j = i - 1,...,0$ 计算得到最小的 $dp[i]$ , 同时用两个变量 $nowHeight$ 和 $nowWidth$ 维护该层最大高度与剩余宽度.
代码如下:
```C++
class Solution {
public:
    int minHeightShelves(vector<vector<int>>& books, int shelfWidth) {
        int n = books.size();
        vector<int> dp(n + 1, 1000000009);
        dp[0] = 0;
        for (int i = 1; i <= n; i++) {
            int nowWidth = shelfWidth;
            int nowHeight = 0;
            for (int j = i - 1; j >= 0; j--) {
                nowWidth -= books[j][0];
                nowHeight = max(nowHeight, books[j][1]);
                if (nowWidth >= 0) {
                    dp[i] = min(dp[i], dp[j] + nowHeight);
                }
                else {
                    break;
                }
            }
        }
        return dp[n];
    }
};
```