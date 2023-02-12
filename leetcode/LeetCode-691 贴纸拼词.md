---
title: LeetCode-691 贴纸拼词 
date: 2022-05-14 16:08:00
updated:
tags: [计算机, 算法, LeetCode, C++, 动态规划, 状态压缩, 记忆化搜索]
categories: LeetCode题解
cover: https://raw.githubusercontent.com/wsylab/wsyPicGo/main/img/photo-1572375992501-4b0892d50c69
description: LeetCode-691「贴纸拼词」的思考与解答
---
### [LeetCode-691 贴纸拼词](https://leetcode.cn/problems/stickers-to-spell-word/)

### Solution 1
这道题要用给定的字符串中的字符来拼出目标字符串, 其中给定字符串中每一个都可以使用无数次.
由题意, 对于每个字符串, 我们可以使用一个数组记录其可以提供的字符种类及个数. 首先思考贪心做法是否可以, 但没能找到解决办法. 再考虑动态规划, 对于一个给定的字符串, 其所需步骤为 $min(f(sticker) + 1), for\ each\ sticker\ in\ stickers[]$ .那么怎样记录状态呢? 一个办法是用 $1$ 表示当前字符已经处理完, $0$ 表示当前字符需要被处理, 这样我们就可以用 $0,1,...,2^n-1$ 来表示字符串拼接过程中所有可能的状态. 由于拼接过程的特性, 我们倾向于使用自顶向下递归的做法来处理状态转移, 通过记忆化搜索避免重复运算, 这一递归过程本质上是广度优先搜索.
代码如下:
```C++
class Solution {
public:
    int minStickers(vector<string>& stickers, string target) {
        int n = target.size();
        vector<int>dp(1 << n, -1); // 赋值, 未被处理的为-1
        dp[0] = 0;
        int ans = recursive(dp, stickers, (1 << n) - 1, target);
        return ans > n ? -1: ans; // 如果ans>n说明没有可行方案, 返回-1
    }

    // 记忆化搜索, 注意数组dp要引用, 否则值不会传回去
    int recursive(vector<int>& dp, vector<string>& stickers, int mask, string target) {
        // 记忆化搜索, 如果之前搜索到这一步了, 直接返回答案即可
        // 这一情况也包含了边界情况dp[0], 我们在调用递归函数之前已经给dp[0]赋值为0了
        if (dp[mask] != -1) {
            return dp[mask];
        }
        dp[mask] = target.size() + 1; // mask如果可以处理完, 那么至多target.size()步可以处理完
        for (string sticker: stickers) {
            int left = mask; // left记录当前sticker处理完后剩余的状态
            vector<int> cnt(26, 0);
            // 记录可用字符
            for (char c: sticker) {
                cnt[c - 'a']++;
            }
            for (int i = 0; i < target.size(); i++) {
                // mask对应的字符串如果未被处理, 且我们有剩余字符可以处理它, 就进行处理.
                if ((mask >> i & 1) && cnt[target[i] - 'a'] > 0) {
                    left = left ^ (1 << i); // left二进制下对应位归0
                    cnt[target[i] - 'a']--; // 可用字符减少
                }
            }
            // 处理后的left必须比mask小
            if (left < mask) {
                dp[mask] = min(dp[mask], recursive(dp, stickers, left, target) + 1); // 状态转移方程
            }
        }
        return dp[mask];
    }
};
```
