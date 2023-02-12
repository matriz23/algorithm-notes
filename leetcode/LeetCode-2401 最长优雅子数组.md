---
title: LeetCode-2401 最长优雅子数组 
date: 2022-09-21 11:41:00
updated:
tags: [计算机, 算法, LeetCode, C++]
categories: LeetCode题解
cover: https://raw.githubusercontent.com/wsylab/wsyPicGo/main/img/photo-1561470872-2e4b48435150
description: LeetCode-2401「最长优雅子数组」的思考与解答
---
### [LeetCode-2401 最长优雅子数组](https://leetcode.cn/problems/longest-nice-subarray/)

### Solution 1
一个优雅子数组要求元素两两之间的与都为 $0$ , 这意味着二进制上整个数组每一位至多有 $1$ 个 $1$ . 注意到对于合法区间 $[l, r]$ , 可以推出 $[l + 1, r]$ 也是合法的, 因此 $r$ 不减. 因此我们遍历 $l$ , 同时维护一个区间与和 $and\_ sum$ , 不断更新 $r$ 即可. 因为要求元素之间两两与为 $0$ , 所以去掉 $nums[l]$ 更新 $and\_ sum$ 只需要令 $and\_ sum = and\_ sum - nums[l]$ .
代码如下:
```C++
class Solution {
public:
    int longestNiceSubarray(vector<int>& nums) {
        int ans = 1;
        int n = nums.size();
        int and_sum = 0;
        int r = 0;
        for (; r < n;) {
            if ((and_sum & nums[r]) == 0) {
                and_sum = and_sum | nums[r];
                r++;
            }
            else {
                break;
            }
        }
        // cout<<0<<", "<<r<<": "<<and_sum<<endl;
        ans = max(ans, r - 0);
        for (int i = 1; i < n; i++) {
            and_sum = and_sum ^ nums[i - 1];
            for (; r < n;) {
                if ((and_sum & nums[r]) == 0) {
                    and_sum = and_sum | nums[r];
                    r++;
                }
                else {
                    break;
                }
            }
            ans = max(ans, r - i);
        }
        return ans;
    }
};
```