---
title: LeetCode-1793 好子数组的最大分数 
date: 2022-09-08 13:05:00
updated:
tags: [计算机, 算法, LeetCode, C++]
categories: LeetCode题解
cover: https://raw.githubusercontent.com/wsylab/wsyPicGo/main/img/photo-1661897906179-18267166aa1a.avif
description: LeetCode-1793「好子数组的最大分数」的思考与解答
---
### [LeetCode-1793 好子数组的最大分数](https://leetcode.cn/problems/maximum-score-of-a-good-subarray/)

### Solution 1
单调栈处理出以 $nums[i]$ 为最小值的左右边界, 枚举可能的情况即可.
代码如下:
```C++
class Solution {
public:
    int maximumScore(vector<int>& nums, int k) {
        int n = nums.size();
        stack<int> st;
        vector<int> left(n);
        for (int i = 0; i < n; i++) {
            while (!st.empty() && nums[st.top()] >= nums[i]) {
                st.pop();
            }
            left[i] = st.empty()? -1: st.top();
            st.push(i);
        }
        st = stack<int>();
        vector<int> right(n);
        for (int i = n - 1; i >= 0; i--) {
            while (!st.empty() && nums[st.top()] >= nums[i]) {
                st.pop();
            }
            right[i] = st.empty()? n: st.top();
            st.push(i);
        }
        int ans = 0;
        for (int i = 0; i < n; i++) {
            if (left[i] < k && right[i] > k) {
                ans = max(ans, nums[i] * (right[i] - left[i] - 1));
            }
        }
        return ans;
    }
};
```