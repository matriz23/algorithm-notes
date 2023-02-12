---
title: LeetCode-410 分割数组的最大值 
date: 2022-09-06 20:14:00
updated:
tags: [计算机, 算法, LeetCode, C++, 动态规划]
categories: LeetCode题解
cover: https://raw.githubusercontent.com/wsylab/wsyPicGo/main/img/photo-1661024206682-736684318f6d.avif
description: LeetCode-410「分割数组的最大值」的思考与解答
---
### [LeetCode-410 分割数组的最大值](https://leetcode.cn/problems/split-array-largest-sum/)

### Solution 1
记 $dp[i][j]$ 为 $0,...,i$ 位元素分割成 $j$ 个非空连续子数组, 各子数组的元素和最大值的最小值. 对于 $dp[i][j]$ , 假设最后一段子数组为 $nums[k + 1], ....,
 nums[i]$ , 那么则有 
$$
dp[i][j] = \underset{j - 2\leq k < i}{min}\{max\{dp[k][j - 1], \sum_{k < t\leq i}nums[t]\}\}
$$
代码如下:
```C++
class Solution {
public:
    int splitArray(vector<int>& nums, int m) {
        int n = nums.size();
        vector<vector<int>> dp(n, vector<int>(m + 1, 1e9 + 10)); 
        vector<int> pre(n, 0);
        for (int i = 0; i < n; i++) {
            pre[i] = (i == 0)? nums[i]: nums[i] + pre[i - 1];
        }
        dp[0][1] = nums[0];
        for (int i = 1; i < n; i++) {
            dp[i][1] = pre[i];
            for (int j = 2; j <= m; j++) {
                for (int k = i - 1; k >= j - 2; k--) {
                    dp[i][j] = min(dp[i][j], max(dp[k][j - 1], pre[i] - pre[k]));
                }
            }
        }
        return dp[n - 1][m];
    }
};
```


### Solution 2
对于一个上界 $k$ , 我们可以在线性时间内验证是否存在合法的划分, 二分搜索这个上界即可.
代码如下:
```C++
class Solution {
public:
    bool check(vector<int>& nums, int m, int k) {
        int cnt = 1;
        int res = 0;
        for (auto num: nums) {
            if (res + num > k) {
                res = num;
                if (res > k) {
                    return false;
                }
                cnt++;
            }
            else {
                res += num;
            }
        }
        return (cnt <= m);
    }

    int splitArray(vector<int>& nums, int m) {
        int n = nums.size();
        int left = 0;
        int right = 1;
        for (auto num: nums) {
            right += num;
        }
        while (left < right) {
            int mid = left + (right - left) / 2;
            if (check(nums, m, mid)) {
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