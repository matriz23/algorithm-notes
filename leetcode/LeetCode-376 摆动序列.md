---
title: LeetCode-376 摆动序列 
date: 2022-05-06 16:49:00
updated:
tags: [计算机, 算法, LeetCode, C++, 动态规划]
categories: LeetCode题解
cover: https://raw.githubusercontent.com/wsylab/wsyPicGo/main/img/photo-1549880338-65ddcdfd017b
description: LeetCode-376「摆动序列」的思考与解答
---
### [LeetCode-376 摆动序列](https://leetcode.cn/problems/wiggle-subsequence/)
直入主题, 这道题可以用动态规划解决, 不过这次我们先看代码, 再来思考状态转移方程的意义.
### Solution 1

```C++
class Solution {
public:
    int wiggleMaxLength(vector<int>& nums) {
        int len = nums.size();
        int dp[1001][2]; 
        dp[0][0] = 1;
        dp[0][1] = 1;
        for (int i = 1; i < len; i++) {
            if (nums[i] > nums[i - 1]) {
                dp[i][0] = max(dp[i - 1][1] + 1, dp[i - 1][0]);
                dp[i][1] = dp[i - 1][1];
            }
            else if (nums[i] < nums[i - 1]) {
                dp[i][0] = dp[i - 1][0];
                dp[i][1] = max(dp[i - 1][0] + 1, dp[i - 1][1]);
            }
            else {
                dp[i][0] = dp[i - 1][0];
                dp[i][1] = dp[i - 1][1];
            }
        }
        return max(dp[len - 1][0], dp[len - 1][1]);
    }
};
```
一个直觉是 $dp[i][0]$ 代表了以 $nums[i]$ 为结尾的最长上升摆动序列长度, $dp[i][1]$ 代表了以 $nums[i]$ 为结尾的最长下降摆动序列长度, 然后考虑状态转移方程.
这种一种典型的错误, 原题意义下的 $dp[i][0]$ 未必由 $dp[i - 1][0]$ 直接"接上" $nums[i]$ 得来. 事实上, 这种推导方式适用于**连续子序列**问题, 很可惜这一题并不是这样的, $dp[i][0]$ 的倒数第二位未必是 $nums[i - 1]$.
让我们重新思考 $dp[i][j]$ 数组的含义, 下面我们以 $dp[i][0]$ 为例( $j = 1$ 时完全对称): 
首先, 我们认识到一开始的意义设定是不便于推导状态转移方程的. 考虑把 $dp[i][0]$ 的意义修改为"以$nums[0],...,nums[i]$中某一元素为结尾的最长上升摆动序列长度" . 那么状态 $dp[i][0]$ 与 $dp[i- 1][0]$ 之间可能的变化因素就是以 $nums[i]$ 为结尾的上升子序列. 
接下来让我们来推导状态转移方程. 
如果 $nums[i] \leq nums[i - 1]$, 考虑变化因素, 对于任何一个 $nums[i]$ 结尾的上升序列, 把 $nums[i]$ 替换成 $nums[i - 1]$ 同样是一个上升序列, 而这部分已经囊括在 $dp[i - 1][0]$ 中了, 故有 $dp[i][0] = dp[i - 1][0]$ ;
再考虑 $nums[i] > nums[i - 1]$ , 一方面, $dp[i][0]$ 可能与 $dp[i - 1][0]$相等; 另一方面, 考虑变化因素, 我们需要寻找 $dp[i - 1][1]$ 接上 $nums[i]$ 后带来的可能性, 接下来我们需要考虑的问题是, 是否一定存在这样的最长下降子序列, 其末尾值小于 $nums[i]$ (只有这样, 才能把 $nums[i]$ 接上)?
答案是肯定的. 我们假定对于 $dp[i - 1][1]$ 意义下的每一个最长下降子序列都满足末尾值(不妨记作 $nums[j]$ )都 $\geq$ $nums[i]$ , 自然有 $nums[j] > nums[i - 1]$ , 这时我们把 $nums[j]$ 用 $nums[i - 1]$ 替换, 就得到了一个同样长的下降子序列, 这个子序列却不符合假设(因为有 $nums[i] > nums[i - 1]$ ), 故假设不成立, 即必能找到一个长度为 $dp[i - 1][1]$ 的下降子序列, 后面能接上 $nums[i]$ 成为一个长度为 $dp[i - 1][1] + 1$ 的上升子序列. 这一情况对应的状态转移方程为 $dp[i][1] = max(dp[i - 1][0] + 1, dp[i - 1][1])$ . 这样我们就圆满地解决这道题了.