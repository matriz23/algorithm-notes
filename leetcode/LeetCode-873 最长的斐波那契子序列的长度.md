---
title: LeetCode-873 最长的斐波那契子序列的长度 
date: 2022-07-09 13:14:00
updated:
tags: [计算机, 算法, LeetCode, C++, 动态规划, 哈希表]
categories: LeetCode题解
cover: https://raw.githubusercontent.com/wsylab/wsyPicGo/main/img/photo-1494935362342-566c6d6e75b5
description: LeetCode-873「最长的斐波那契子序列的长度」的思考与解答
---
### [LeetCode-873 最长的斐波那契子序列的长度](https://leetcode.cn/problems/length-of-longest-fibonacci-subsequence/)

### Solution 1
对于这种最长子序列的问题, 我们一般以其末尾的元素定义状态. 对于这道题, 有一点需要注意的是: 斐波那契子序列的状态推导和最后两个元素密切相关, 因此我们定义 $dp$ 数组时, 应该定义二维数组进行状态转移. 定义 $dp[i][j]$ 为以 $arr[j], arr[i]$ 为结尾的斐波那契子序列的最长长度, 则有如下状态转移方程:
$$
dp[i][j]=\underset{arr[k] = arr[i] - arr[j]}{max}dp[j][k] + 1
$$
由于数组单调的特点, 我们可以建立哈希表, 快速找到一个值对应的下标.
代码如下:
```C++
class Solution {
public:
    int lenLongestFibSubseq(vector<int>& arr) {
        int INF = -(1e9 + 7);
        int n = arr.size();
        vector<vector<int>> dp(n, vector<int>(n, INF));
        map<int, int> idx;
        for (int i = 0; i < n; i++) {
            idx[arr[i]] = i;
        }
        for (int i = 1; i < n; i++) {
            for (int j = 0; j < i; j++) {
                dp[i][j] = 2;
            }
        }
        int ans = 2;
        for (int i = 2; i < n; i++) {
            for (int j = 1; j < i; j++) {
                if (idx.count(arr[i] - arr[j])) {
                    dp[i][j] = max(dp[i][j], dp[j][idx[arr[i] - arr[j]]] + 1);
                }
                ans = max(ans, dp[i][j]);
            }
        }
        return ans > 2? ans: 0;
    }
};
```