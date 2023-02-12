---
title: LeetCode-473 火柴拼正方形 
date: 2022-08-02 10:33:00
updated:
tags: [计算机, 算法, LeetCode, C++, 递归, 回溯]
categories: LeetCode题解
cover: https://raw.githubusercontent.com/wsylab/wsyPicGo/main/img/photo-1515283736202-cbe98351a5d8
description: LeetCode-473「火柴拼正方形」的思考与解答
---
### [LeetCode-473 火柴拼正方形](https://leetcode.cn/problems/matchsticks-to-square/)

### Solution 1
回溯算法, 具体的细节见代码注释.
```C++
class Solution {
public:
    int edges[4]; // edges 记录正方形的四个边
    int len = 0; // 记录每条边的长度

    bool dfs(int index, vector<int>& matchsticks) { // 递归函数, 考虑第 index 个边的放置能否符合要求
        if (index == matchsticks.size()) { // 如果已经放完了, 说明可以拼成正方形, 返回 true
            return true;
        }
        for (int i = 0; i < 4; i++) { // 还没有放完, 依次考虑四条边
            edges[i] += matchsticks[index]; // 每条边先加上当前火柴长度
            if (edges[i] <= len && dfs(index + 1, matchsticks)) { // 如果当前边长度 <= len, 还可以继续放, 如果后续的放置满足要求, 则满足要求, 返回 true
                return true;
            }
            edges[i] -= matchsticks[index]; // 回溯, 撤销前面的操作
        }
        return false; // 一直没能找到合法的方案, 返回 false
    }

    bool makesquare(vector<int>& matchsticks) {
        for (int match: matchsticks) {
            len += match;
        }
        if (len % 4 != 0) { // 先计算总长度, 应当是 4 的倍数
            return false;
        }
        len /= 4;
        memset(edges, 0, sizeof(edges));
        sort(matchsticks.begin(), matchsticks.end(), greater<int>()); // 先从大的火柴棍开始试, 减少搜索量
        return dfs(0, matchsticks);
    }
};
```
