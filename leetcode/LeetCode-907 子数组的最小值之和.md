---
title: LeetCode-907 子数组的最小值之和 
date: 2022-10-31 14:44:00
updated:
tags: [计算机, 算法, LeetCode, C++, 单调栈]
categories: LeetCode题解
cover: 
description: LeetCode-907「子数组的最小值之和」的思考与解答
---
### [LeetCode-907 子数组的最小值之和](https://leetcode.cn/problems/sum-of-subarray-minimums/)

### Solution 1
常见的考虑方法是对每个元素作为最小值的情况计数. 为了避免重复计数, 对于值相同的元素, 我们认为下标更小的那个元素更小.
代码如下:
```C++
#define ll long long
const int MOD = 1e9 + 7;
class Solution {
public:
    int sumSubarrayMins(vector<int>& nums) {
        int n = nums.size();
        stack<int> s;
        vector<int> left(n);
        for (int i = 0; i < n; i++) {
            while (!s.empty() && nums[s.top()] > nums[i]) {
                s.pop();
            }
            left[i] = s.empty()? -1: s.top();
            s.push(i);
        }
        s = stack<int>();
        vector<int> right(n);
        for (int i = n - 1; i >= 0; i--) {
            while (!s.empty() && nums[s.top()] >= nums[i]) {
                s.pop();
            }
            right[i] = s.empty()? n: s.top();
            s.push(i);
        }
        ll ans = 0;
        for (int i = 0; i < n; i++) {
            ans = (ans + (ll)nums[i] * (i - left[i]) * (right[i] - i)) % MOD;
        }
        return ans;
    }
};
```