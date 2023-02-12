---
title: LeetCode-1723 完成所有工作的最短时间 
date: 2022-09-16 10:35:00
updated:
tags: [计算机, 算法, LeetCode, C++, 动态规划, 回溯]
categories: LeetCode题解
cover: https://raw.githubusercontent.com/wsylab/wsyPicGo/main/img/photo-1625974167082-c5080eafe61e.avif
description: LeetCode-1723「完成所有工作的最短时间」的思考与解答
---
### [LeetCode-1723 完成所有工作的最短时间](https://leetcode.cn/problems/find-minimum-time-to-finish-all-jobs/)

### Solution 1
枚举的过程中, 第 $i$ 个工人面对的工作只与前 $i - 1$ 个工人的工作分配情况相关. 如果我们用 $dp[i][j]$ 表示前 $0, ..., i$ 这些工人承担了集合 $j$ 表示的工作, 则有状态转移方程:
$$
dp[i][j] = \underset{k\in j}{min\{max(dp[i - 1][k], sum[j - k])\}} 
$$
其中 $sum$ 为集合表示的工作的工作量之和, 可以预处理得到.
代码如下:
```C++
class Solution {
public:
    int minimumTimeRequired(vector<int>& jobs, int k) {
        int n = jobs.size();
        int s = (1 << n) - 1;
        vector<int> sum(s + 1, 0);
        vector<vector<int>> dp(n, vector<int>(s + 1, 0));
        for (int i = 1; i <= s; i++) {
            int j = (i - 1) & i;
            sum[i] = sum[j] + sum[i ^ j];
            dp[0][i] = sum[i];
        }
        for (int i = 1; i < n; i++) {
            for (int j = 1; j <= s; j++) {
                dp[i][j] = 1e9;
                for (int k = j; k >= 0; k = (k - 1) & j) {
                    dp[i][j] = min(dp[i][k], sum[j ^ k]);
                }
            }
        }
        return dp[n - 1][s];
    }
};
```

### Solution
求最大值的最小值, 考虑二分. $check$ 函数利用回溯, 枚举可能的放置情况一步步判断.
代码如下:
```C++
class Solution {
public:
    vector<int> _jobs;
    int _k;

    bool backTrack(vector<int> workers, int m, int idx) {
        if (idx == _jobs.size()) {
            return true;
        }
        for (auto& worker: workers) {
            if (worker + _jobs[idx] <= m) {
                worker += _jobs[idx];
                if (backTrack(workers, m, idx + 1)) {
                    return true;
                }
                worker -= _jobs[idx];
            }
            if (worker == 0 || worker + _jobs[idx] == m) {
                break;
            }
        }
        return false;
    }

    bool check(int m) {
        vector<int> workers(_k, 0);
        return backTrack(workers, m, 0);
    }

    int minimumTimeRequired(vector<int>& jobs, int k) {
        sort(jobs.begin(), jobs.end(), greater<int>());
        _jobs = jobs;
        _k = k;
        int left = jobs[0];
        int right = accumulate(jobs.begin(), jobs.end(), 0);
        while (left < right) {
            int mid = left + (right - left) / 2;
            if (check(mid)) {
                right = mid;
            }
            else {
                left = mid + 1;
            }
        }
        return left;
    }
};
```