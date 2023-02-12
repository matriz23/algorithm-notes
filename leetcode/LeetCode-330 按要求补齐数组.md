---
title: LeetCode-330 按要求补齐数组 
date: 2022-09-06 11:40:00
updated:
tags: [计算机, 算法, LeetCode, C++, 数学]
categories: LeetCode题解
cover: https://raw.githubusercontent.com/wsylab/wsyPicGo/main/img/photo-1464002255390-2de63a26c637.jpg
description: LeetCode-330「按要求补齐数组」的思考与解答
---
### [LeetCode-330 按要求补齐数组](https://leetcode.cn/problems/patching-array/)

### Solution 1
如果能覆盖 $[1, x - 1]$ 中的数, 而不能覆盖 $x$ , 那么加上 $x$ 之后一定能够覆盖 $[1, 2x - 1]$ 中的数, 并且这是最优的. 我们要做的就是不断寻找第一个覆盖不到的 $x$ . 如果 $nums[0, ..., i - 1]$ 可以覆盖 $[1, x - 1]$ , 考虑 $nums[i]$ , 如果 $nums[i] <= x$ , 那么 $nums[0, ..., i]$ 可以覆盖 $1, x + nums[i] - 1]$ ; 如果 $nums[i] > x$ , 那么需要补充 $x$ , 覆盖 $[1, 2x - 1]$ .
代码如下:
```C++
class Solution {
public:
    int minPatches(vector<int>& nums, int n) {
        int ans = 0;
        long long x = 1;
        int idx = 0;
        while (x <= n) {
            if (idx < nums.size() && nums[idx] <= x) {
                x += nums[idx];
                idx++;
            } else {
                x *= 2;
                ans++;
            }
        }
        return ans;
    }
};
```