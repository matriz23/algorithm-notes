---
title: LeetCode-2369 检查数组是否存在有效划分 
date: 2022-08-18 18:04:00
updated:
tags: [计算机, 算法, LeetCode, C++, 动态规划]
categories: LeetCode题解
cover: https://raw.githubusercontent.com/wsylab/wsyPicGo/main/img/photo-1509565840034-3c385bbe6451
description: LeetCode-2369「检查数组是否存在有效划分」的思考与解答
---
### [LeetCode-2369 检查数组是否存在有效划分](https://leetcode.cn/problems/check-if-there-is-a-valid-partition-for-the-array/)

### Solution 1
划分方式和 "爬楼梯" 问题很相似, 我们用 $dp[i]$ 记录前 $i + 1$ 个元素能否产生有效划分, 根据 $nums[i], nums[i - 1], nums[i - 2]$ 的关系进行递推即可.
代码如下:
```C++
class Solution {
public:
    bool validPartition(vector<int>& nums) {
        int n = nums.size();
        vector<bool> dp(n, false);
        dp[0] = false;
        dp[1] = (nums[1] == nums[0]);
        if (n == 2) {
            return dp[1];
        }
        dp[2] = (nums[2] == nums[1] && nums[1] == nums[0]) || (nums[2] == nums[1] + 1 && nums[1] == nums[0] + 1);
        for (int i = 3; i < n; i++) {
            if (nums[i] == nums[i - 1]) {
                dp[i] = dp[i] + dp[i - 2];
                if (nums[i - 1] == nums[i - 2]) {
                    dp[i] = dp[i] + dp[i - 3];
                }
            }
            else if (nums[i] == nums[i - 1] + 1 && nums[i - 1] == nums[i - 2] + 1) {
                dp[i] = dp[i] + dp[i - 3];
            }
        }
        return dp[n - 1];
    }
};
```