---
title: LeetCode-798 得分最高的最小轮调 
date: 2022-09-16 15:51:00
updated:
tags: [计算机, 算法, LeetCode, C++]
categories: LeetCode题解
cover: https://raw.githubusercontent.com/wsylab/wsyPicGo/main/img/photo-1603033086926-1e8c85dadd54.avif
description: LeetCode-798「得分最高的最小轮调」的思考与解答
---
### [LeetCode-798 得分最高的最小轮调](https://leetcode.cn/problems/smallest-rotation-with-highest-score/)

### Solution 1
与其考虑每个 $k$ 能获得多少分, 不如考虑每个 $nums[i]$ 能在哪些 $k$ 下得分. 基于这一点, 我们可以算出 $nums[i]$ 对应的 $k$ 的区间. 考虑这段区间里的 $k$ 得分 $+1$ . 最终答案即为得分最高的那个 $k$ . 区间操作, 单次查询, 我们可以使用差分数组来处理这个过程.
代码如下:
```C++
class Solution {
public:
    int bestRotation(vector<int>& nums) {
        int n = nums.size();
        vector<int> diff(2 * n + 1, 0);
        for (int i = 0; i < n; i++) {
            int x = nums[i];
            if (i >= x) {
                diff[0]++;
                diff[i - x + 1]--;
                diff[i + 1]++;
                diff[n]--;
            }
            else {
                diff[i + 1]++;
                diff[n + i - x + 1]--;
            }
        }
        int maxn = diff[0];
        int ans = 0;
        for (int i = 1; i < n; i++) {
            diff[i] += diff[i - 1];
            if (diff[i] > maxn) {
                ans = i;
                maxn = diff[i];
            }
        }
        return ans;
    }
};
```