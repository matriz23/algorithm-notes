---
title: LeetCode-2560 打家劫舍 IV
date: 2023-02-09 23:16:00
updated:
tags: [计算机, 算法, LeetCode, C++, 贪心, 二分]
categories: LeetCode题解
cover: 
description: LeetCode-2560「打家劫舍 IV」的思考与解答
---
### [LeetCode-2560 打家劫舍 IV](https://leetcode.cn/problems/house-robber-iv/)

### Solution 1
求每次窃取过程中最大金额的最小值, 注意到如果 $k$ 可行, 那么 $k+1$ 同样可行, 并且可以在线性时间内判断一个解是否可行, 因此用二分搜索寻找左边界.
一次窃取过程中最大金额的最小值一定是某个房屋的价值, 可以证明二分搜索的结果一定是某个房屋的价值.
代码如下:
```C++
class Solution {
   public:
    vector<int> nums;
    int k;

    bool check(int v) { // 贪心, 尽可能选靠左的房屋
        int cnt = k;
        for (int i = 0; i < nums.size(); i++) {
            if (nums[i] <= v) {
                cnt--;
                i++;
            }
            if (cnt == 0) {
                return true;
            }
        }
        return false;
    }

    int minCapability(vector<int>& _nums, int _k) {
        nums = _nums;
        k = _k;
        int l = 0;
        int r = 1e9 + 10;
        while (l < r) {
            int m = l + (r - l) / 2;
            if (check(m)) {
                r = m;
            } 
            else {
                l = m + 1;
            }
        }
        return l;
    }
};
```