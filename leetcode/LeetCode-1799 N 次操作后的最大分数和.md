---
title: LeetCode-1799 N 次操作后的最大分数和 
date: 2022-12-22 11:11:00
updated:
tags: [计算机, 算法, LeetCode, C++, 动态规划]
categories: LeetCode题解
cover: 
description: LeetCode-1799「N 次操作后的最大分数和」的思考与解答
---
### [LeetCode-1799 N 次操作后的最大分数和](https://leetcode.cn/problems/maximize-score-after-n-operations/)

### Solution 1
状压 DP 即可.
代码如下:
```C++
class Solution {
   public:
    int memo[20000];
    int g[14][14];
    int f(int s, vector<int>& nums) {
        if (s == 0) {
            return 0;
        }
        if (memo[s] != -1) {
            return memo[s];
        }
        int n = nums.size();
        int id = (n - __builtin_popcount(s)) / 2 + 1;
        int res = 0;
        for (int i = 0; i < n; i++) {
            if (s & 1 << i) {
                for (int j = i + 1; j < n; j++) {
                    if (s & 1 << j) {
                        res = max(res, f(s - (1 << i) - (1 << j), nums) + id * g[i][j]);
                    }
                }
            }
        }
        memo[s] = res;
        return res;
    }

    int maxScore(vector<int>& nums) {
        int n = nums.size();
        for (int i = 0; i < 1 << n; i++) {
            memo[i] = -1;
        }
        for (int i = 0; i < n; i++) {
            for (int j = i + 1; j < n; j++) {
                g[i][j] = __gcd(nums[i], nums[j]);
            }
        }
        return f((1 << n) - 1, nums);
    }
};
```