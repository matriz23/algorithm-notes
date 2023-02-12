---
title: LeetCode-42 接雨水
date: 2022-08-30 15:46:00
updated:
tags: [计算机, 算法, LeetCode, C++, 动态规划]
categories: LeetCode题解
cover: https://raw.githubusercontent.com/wsylab/wsyPicGo/main/img/photo-1517964101322-a4cfaaf56e5b.avif
description: LeetCode-42「接雨水」的思考与解答
---
### [LeetCode-42 接雨水](https://leetcode.cn/problems/trapping-rain-water/)

### Solution 1
一块地方承载的雨水量由自身高度和左侧最大高度、右侧最大高度共同决定. 用一次遍历计算出每一块两侧的最大高度, 再用一次遍历计算出 $ans$ .
代码如下:
```C++
class Solution {
public:
    int trap(vector<int>& height) {
        int ans = 0;
        int n = height.size();
        vector<int> left(n, 0), right(n, 0);
        stack<int> st;
        for (int i = 1; i < n; i++) {
            left[i] = max(left[i - 1], height[i - 1]);
            right[n - 1 - i] = max(right[n - i], height[n - i]);
        }
        for (int i = 0; i < n; i++) {
            if (left[i] >= height[i] && right[i] >= height[i]) {
                ans += min(left[i], right[i]) - height[i];
            }
        }
        return ans;
    }
};
```