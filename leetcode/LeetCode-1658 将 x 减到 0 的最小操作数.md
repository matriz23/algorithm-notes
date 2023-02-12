---
title: LeetCode-1658 将 x 减到 0 的最小操作数 
date: 2022-08-24 08:26:00
updated:
tags: [计算机, 算法, LeetCode, C++, 二分, 滑动窗口, 双指针]
categories: LeetCode题解
cover: https://raw.githubusercontent.com/wsylab/wsyPicGo/main/img/photo-1507703723435-d6a869ff0f8c
description: LeetCode-1658「将 x 减到 0 的最小操作数」的思考与解答
---
### [LeetCode-1658 将 x 减到 0 的最小操作数](https://leetcode.cn/problems/minimum-operations-to-reduce-x-to-zero/)

### Solution 1
根据题意, 二分寻找和为 $x$ 的前缀和与后缀和.
代码如下:
```C++
class Solution {
public:
    int minOperations(vector<int>& nums, int x) {
        int n = nums.size();
        vector<int> pre(n, 0), suf(n, 0);
        int ans = 0x3f3f3f3f;
        for (int i = 0; i < n; i++) {
            pre[i] = (i == 0)? nums[i]: nums[i] + pre[i - 1];
            suf[n - 1 - i] = (i == 0)? nums[n - 1 - i]: nums[n - 1 - i] + suf[n - i];
            if (pre[i] == x || suf[n - 1 - i] == x) {
                ans = min(ans, i + 1);
            }
        }        
        for (int i = 1; i < n; i++) {
            int left = i + 1;
            int right = n - 1;
            int target = x - pre[i];
            while (left <= right) {
                int mid = left + (right - left) / 2;
                if (suf[mid] == target) {
                    ans = min(ans, n - mid + i + 1);
                    break;
                }
                else if (suf[mid] < target) {
                    right = mid - 1;
                }
                else {
                    left = mid + 1;
                }
            }
        }
        return ans == 0x3f3f3f3f? -1: ans;
    }
};
```

### Solution 2
记数组和为 $sum$ , 从数组两侧取走和为 $x$ 的数, 那么留下来的一定是一段和为 $sum - x$ 的连续子数组. 用滑动窗口找出符合条件的最长连续子数组即可.
代码如下:
```C++
class Solution {
public:
    int minOperations(vector<int>& nums, int x) {
        int n = nums.size();
        int sum = 0;
        for (auto num: nums) {
            sum += num;
        }
        sum -= x;
        cout<<"sum = "<<sum<<endl;
        if (sum < 0) {
            return -1;
        }
        else if (sum == 0){
            return n;
        }
        int ans = 0x3f3f3f3f;
        int left = 0, right = 0;
        int win_sum = 0;
        while (right < n) {
            win_sum += nums[right];
            right++;
            while (win_sum > sum) {
                win_sum -= nums[left];
                left++;
            }
            if(win_sum == sum) {
                ans = min(ans, n - right + left);
            }
        }
        return ans == 0x3f3f3f3f? -1: ans;
    }
};
```