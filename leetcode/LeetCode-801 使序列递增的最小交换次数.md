---
title: LeetCode-801 使序列递增的最小交换次数 
date: 2022-09-16 16:22:00
updated:
tags: [计算机, 算法, LeetCode, C++, 动态规划]
categories: LeetCode题解
cover: https://raw.githubusercontent.com/wsylab/wsyPicGo/main/img/photo-1651099799036-0da7f1ad685e-1.avif
description: LeetCode-801「使序列递增的最小交换次数」的思考与解答
---
### [LeetCode-801 使序列递增的最小交换次数](https://leetcode.cn/problems/minimum-swaps-to-make-sequences-increasing/)

### Solution 1
可以先考虑二分 + 贪心, 不过容易验证贪心是错的. 因为每一位有交换与不交换两种操作, 不妨考虑动态规划. 因为是否交换、交换完的序列是否合法与上一次是否交换有关, 因此我们用 $dp[i][j]$ 记录前 $i + 1$ 个字符, 最后一个字符是否交换, 形成递增序列的最少交换次数. $dp[i][j]$ 可以在一定条件下从 $dp[i - 1][0]$ 和 $dp[i - 1][1]$ 转化而来. 具体转移方程见代码.
代码如下:
```C++
class Solution {
public:
    int minSwap(vector<int>& nums1, vector<int>& nums2) {
        int n = nums1.size();
        vector<vector<int>> dp(n, vector<int>(2, 1e9));
        dp[0][0] = 0;
        dp[0][1] = 1;
        for (int i = 1; i < n; i++) {
            if (nums1[i] > nums1[i - 1] && nums2[i] > nums2[i - 1]) {
                dp[i][0] = dp[i - 1][0];
                dp[i][1] = dp[i - 1][1] + 1;
            }
            if (nums1[i] > nums2[i - 1] && nums2[i] > nums1[i - 1]) {
                dp[i][0] = min(dp[i][0], dp[i - 1][1]);
                dp[i][1] = min(dp[i][1], dp[i - 1][0] + 1);
            }
        }
        return min(dp[n - 1][0], dp[n - 1][1]);
    }
};
```