---
title: LeetCode-2407 最长递增子序列 II 
date: 2022-09-16 15:24:00
updated:
tags: [计算机, 算法, LeetCode, C++, 线段树, 动态规划]
categories: LeetCode题解
cover: 
description: LeetCode-2407「最长递增子序列 II」的思考与解答
---
### [LeetCode-2407 最长递增子序列 II](https://leetcode.cn/problems/longest-increasing-subsequence-ii/)

### Solution 1
记 $dp[x]$ 为当前以 $x$ 值结尾的子序列的最长长度. 遍历数组, 当前元素为 $x$ 时, $dp[x] = max(dp[x - k], ..., dp[x - 1]) + 1$ . 因此我们要能找到一个数据结构, 能够快速地实现以下两种操作:
- 单点修改
- 查询区间最大值

代码如下:
```C++
class Solution {
    int mx;
    vector<int> maxo;

    // 查询区间 [ql, qr] 中的最大值
    int query(int id, int l, int r, int ql, int qr) {
        if (ql <= l && r <= qr) return maxo[id];
        else {
            int nxt = id << 1, mid = (l + r) >> 1;
            return max(ql <= mid ? query(nxt, l, mid, ql, qr) : 0, qr > mid ? query(nxt | 1, mid + 1, r, ql, qr) : 0);
        }
    }

    // 用 val 更新位置 pos 的最大值
    void modify(int id, int l, int r, int pos, int val) {
        if (l == r) maxo[id] = max(maxo[id], val);
        else {
            int nxt = id << 1, mid = (l + r) >> 1;
            if (pos <= mid) {
                modify(nxt, l, mid, pos, val);
            }
            else {
                modify(nxt | 1, mid + 1, r, pos, val);
            }
            maxo[id] = max(maxo[nxt], maxo[nxt | 1]);
        }
    }

public:
    int lengthOfLIS(vector<int>& nums, int k) {
        for (int x : nums) mx = max(mx, x);
        maxo.resize(mx * 4 + 10); // 线段树开四倍大小
        int n = nums.size();
        int ans = 0;
        for (int x: nums) {
            int res;
            if (x == 1) {
                res = 1;
            }
            else {
                res = query(1, 1, mx, max(1, x - k), x - 1) + 1;
            }
            modify(1, 1, mx, x, res);
        }
        return query(1, 1, mx, 1, mx);
    }
};
```