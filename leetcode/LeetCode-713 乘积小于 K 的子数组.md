---
title: LeetCode-713 乘积小于 K 的子数组
date: 2022-05-05 10:33:00
updated:
tags: [计算机, 算法, LeetCode, C++, 滑动窗口]
categories: LeetCode题解
cover: https://raw.githubusercontent.com/wsylab/wsyPicGo/main/img/photo-1464822759023-fed622ff2c3b
description: LeetCode-713「乘积小于 K 的子数组」的思考与解答
---
### [LeetCode-713 乘积小于 K 的子数组](https://leetcode.cn/problems/subarray-product-less-than-k/)

### Solution 1
题目中所求的是满足元素乘积小于 $K$ 的连续子数组的数目, 连续子数组可以考虑动态规划或者滑动窗口. 本题中元素全部为正整数, 符合使用滑动窗口的要求.
我们从最左侧开始, 建立一个长度只有 $1$ 的窗口 $[left=0, right = 0]$, 同时用 $pro$ 来记录当前窗口元素之积. 现在计算以 $nums[right]$ 为结尾的符合要求的连续子数组个数: 每次窗口更新后, 我们通过右移 $left$ 来减少 $pro$ 使 $pro < k$, 由于元素都是正整数, 第一个满足 $pro < k$ 的 $left$ 右侧的所有 $left$ 一定也满足 $pro < k$, 因此以 $nums[right]$ 为结尾的合法子数组个数即为 $right - left + 1$. 把结果累加到 $ans$ 后, $right++$, 继续考虑下一位, 当到达数组末尾时跳出循环.
代码如下:
```C++
class Solution {
public:
    int numSubarrayProductLessThanK(vector<int>& nums, int k) {
        int ans = 0;
        int right = 0;
        int left = 0;
        int pro = 1; 
        while (right < nums.size()) {
            pro = pro * nums[right]; //考虑新窗口, 更新pro
            while (pro >= k && left <= right) {
                pro = pro / nums[left];
                left++;
            }
            ans = ans + right - left + 1;
            right++;
        }
        return ans;
    }
};
```
> 一个需要注意的地方是 $pro$ 何时更新, 把 $pro$ 放在循环开始时更新可以保证 $nums[right]$ 不越界.