---
title: LeetCode-2350 不可能得到的最短骰子序列 
date: 2022-08-23 20:56:00
updated:
tags: [计算机, 算法, LeetCode, C++, 贪心, 思维]
categories: LeetCode题解
cover: https://raw.githubusercontent.com/wsylab/wsyPicGo/main/img/photo-1570303345338-e1f0eddf4946
description: LeetCode-2350「不可能得到的最短骰子序列」的思考与解答
---
### [LeetCode-2350 不可能得到的最短骰子序列](https://leetcode.cn/problems/shortest-impossible-sequence-of-rolls/)

### Solution 1
假设不可能得到的最短骰子序列长度为 $len$ , 那么对于长度为 $
len - 1$ 的骰子序列, 都能从 $rolls$ 中得到. 对于 $k$ 面的骰子, 每一位都有 $k$ 种选择. 考虑第 $1$ 位, $rolls$ 中应当包含 $1, 2, ..., k$ ; 考虑第 $2$ 位, 在第 $1$ 位任意选择后, 第 $2$ 位也能够任意选择 $1, 2, ..., k$ , 说明在第一组 $1, 2, ..., k$ 后还需要有一组 $1, 2, ..., k$ . 依此类推, 我们用一个 $set$ 记录一组完全的骰子数, 统计到一组后开始统计下一组. 假设有 $cnt$ 组, 那么答案就是 $cnt + 1$ .
代码如下:
```C++
class Solution {
public:
    int shortestSequence(vector<int>& rolls, int k) {
        set<int> s;
        int ans = 1;
        for (int roll: rolls) {
            if (!s.count(roll)) {
                s.insert(roll);
            }
            if (s.size() == k) {
                ans++;
                s.clear();
            }
        }
        return ans;
    }
};
```