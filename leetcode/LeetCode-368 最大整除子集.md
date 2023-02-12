---
title: LeetCode-368 最大整除子集 
date: 2022-5-8 19:33:00
updated:
tags: [计算机, 算法, LeetCode, C++, 动态规划, 排序]
categories: LeetCode题解
cover: https://raw.githubusercontent.com/wsylab/wsyPicGo/main/img/photo-1514828260103-1e9bf9a58446
description: LeetCode-368「最大整除子集」的思考与解答
---
### [LeetCode-368 最大整除子集](https://leetcode.cn/problems/largest-divisible-subset/)

<font size=8>**论排序的重要性**</font>
### Solution 1
寻找数组中的最大整除子集, 常规思路中最大的难点在于状态转移方程的条件判定, 对于 $j < i$, 如果 $nums[i]$ 为以 $nums[j]$ 为结尾的最大整除子集中最大值的倍数, 则有 $dp[i] = max(dp[i], dp[j] + 1)$, 但如果不是这样, 我们很难判定加入 $nums[i]$ 后能否构成一个新的整除子集, 因为状态转移方程只告诉我们有几个整除子集, 而不会给出它们各自具体的构造.
能否解决这一困境呢? 
我们觉得 $nums[i]$ 较小的时候难以处理, 那么保证对 $\forall j < i$ 有 $nums[j] < nums[i]$ 不就不用处理这种情况了吗? 把原数组排个序再处理, 这道题变得相当简单.
最后题目要求给出一个最大整除子集的构造, 我们不妨从后向前, 把子集个数从 $dp[index]$ 一路降至 $1$, 从大到小构造出这个子集.
代码如下:
```C++
class Solution {
public:
    vector<int> largestDivisibleSubset(vector<int>& nums) {
        int n = nums.size();
        sort(nums.begin(), nums.end());
        vector<int> dp(n, 1);
        for (int i = 1; i < n; i++) {
            int key = nums[i];
            for (int j = 0; j < i; j++) {
                if (key % nums[j] == 0 && dp[j] + 1 > dp[i]) {
                    dp[i] = dp[j] + 1;
                }
            }
        }
        int max = 0;
        int index = 0;
        for (int i = 0; i < n; i++) {
            if (dp[i] > max) {
                max = dp[i];
                index = i;
            }
        }
        vector<int> ans;
        for (int i = n - 1; i >= 0; i--) {
            if (dp[i] == max && nums[index] % nums[i] == 0) {
                max--;
                index = i;
                ans.push_back(nums[i]);
            }
        }
        return ans;
    }
};
```