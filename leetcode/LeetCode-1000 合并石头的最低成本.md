---
title: LeetCode-1000 合并石头的最低成本 
date: 2022-09-20 15:00:00
updated:
tags: [计算机, 算法, LeetCode, C++, 动态规划]
categories: LeetCode题解
cover: https://raw.githubusercontent.com/wsylab/wsyPicGo/main/img/photo-1532009877282-3340270e0529.avif
description: LeetCode-1000「合并石头的最低成本」的思考与解答
---
### [LeetCode-1000 合并石头的最低成本](https://leetcode.cn/problems/minimum-cost-to-merge-stones/)

### Solution 1
考虑合并为 $1$ 堆的最后一步, 一定是把 $k$ 堆石头合并为了一堆, 并且消耗了所有的石头数量之和的成本. 从区间上考虑问题, 记 $f(i, j, p)$ 为区间 $[i: j]$ 中的所有石头合并为 $p$ 堆石头的最低成本. 我们要求的最终答案就是 $f(0, n - 1, 1)$ . 先考虑一些边界情况, 如果区间内石头堆数量 $j - i + 1 < p$ , 很显然不存在合法的方案 (返回 $-1$ 表示不合法) ; 如果区间内石头堆数量 $j - i + 1 = p$ , 那么不需要合成费用. 对于其他的情况, 如果 $p = 1$ , 这 $1$ 堆需要 $k$ 堆来合成, 故 $f(i, j, 1) = f(i, j, k) + \sum_{i\leq t\leq j}stones[t]$ . 如果 $p > 1$ , 那么考虑最左侧的一堆由左区间提供, 最右侧的 $p - 1$ 堆由右区间提供, 枚举分割点即可.
代码如下:
```C++
class Solution {
public:
    int _k;
    int n;
    vector<int> pre;
    vector<vector<vector<int>>> dp;

    int f(int i, int j, int p) {
        if ((j - i + 1 - p) % (_k - 1)) {
            return -1;
        }
        if (j - i + 1 < p) {
            return -1;
        }
        if (j - i + 1 == p) {
            return 0;
        }
        if (dp[i][j][p] != -1) {
            return dp[i][j][p];
        }
        if (p == 1) {
            dp[i][j][p] = f(i, j, _k) + ((i == 0)? pre[j]: pre[j] - pre[i - 1]); 
            return dp[i][j][p];
        }
        int res = 1e9;
        for (int t = i; t < j; t++) { 
            int f_l = f(i, t, 1);
            int f_r = f(t + 1, j, p - 1);
            if (f_l != -1 && f_r != -1) {
                res = min(res, f_l + f_r);
            }
        }
        dp[i][j][p] = (res == 1e9)? -1: res;
        return dp[i][j][p];
    }

    int mergeStones(vector<int>& stones, int k) {
        int n = stones.size();
        if ((n - 1) % (k - 1)) {
            return -1;
        }
        dp = vector<vector<vector<int>>>(31, vector<vector<int>>(31, vector<int>(31, -1)));
        _k = k;
        pre = vector<int>(n, 0);
        for (int i = 0; i < n; i++) {
            pre[i] = i == 0? stones[i]: pre[i - 1] + stones[i];
        }
        return f(0, n - 1, 1);
    }
};
```