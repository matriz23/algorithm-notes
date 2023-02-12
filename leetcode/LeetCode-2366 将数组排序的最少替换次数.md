---
title: LeetCode-2366 将数组排序的最少替换次数 
date: 2022-08-18 14:24:00
updated:
tags: [计算机, 算法, LeetCode, C++, 贪心]
categories: LeetCode题解
cover: https://raw.githubusercontent.com/wsylab/wsyPicGo/main/img/photo-1461840338307-ceb7b261d6e8
description: LeetCode-2366「将数组排序的最少替换次数」的思考与解答
---
### [LeetCode-2366 将数组排序的最少替换次数](https://leetcode.cn/problems/minimum-replacements-to-sort-the-array/)

### Solution 1
不断把 $nums[i]$ 用 $t$ 步拆分成为 $a_{i1}, a_{i2}, ..., a_{i(t+1)}$ 共 $t + 1$ 个数, 最终得到一个非递减的数组. 容易想到, 后面的数尽可能大的情况下, 前面比较小的数可以不拆, 比较大的数可以少拆. 从这个角度出发, 我们从后向前遍历, 用 $mod$ 维护当前拆分成多大的数, 如果 $nums[i] < mod$ , 把 $mod$ 更新为 $nums[i]$ ; 如果 $nums[i] > mod$ , 观察 $mod$ 是否整除 $nums[i]$ , 如果整除直接拆; 如果不整除, 尽可能拆分成大的数字, 同时更新 $mod$ . 一次遍历即可.
代码如下:
```C++
#define ll long long
class Solution {
public:
    long long minimumReplacement(vector<int>& nums) {
        ll ans = 0;
        int n = nums.size();
        int mod = nums[n - 1];
        for (int i = n - 2; i >= 0; i--) {
            mod = min(mod, nums[i]);
            ans += (nums[i] - 1) / mod;
            if (nums[i] % mod != 0) {
                mod = nums[i] / (((nums[i] - 1) / mod) + 1);
            }
        }
        return ans;
    }
};
```