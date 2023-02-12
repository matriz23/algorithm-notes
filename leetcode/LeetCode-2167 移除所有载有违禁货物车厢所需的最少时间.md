---
title: LeetCode-2167 移除所有载有违禁货物车厢所需的最少时间 
date: 2022-05-13 16:40:00
updated:
tags: [计算机, 算法, LeetCode, C++, 动态规划]
categories: LeetCode题解
cover: https://raw.githubusercontent.com/wsylab/wsyPicGo/main/img/photo-1474487548417-781cb71495f3
description: LeetCode-2167「移除所有载有违禁货物车厢所需的最少时间」的思考与解答
---
### [LeetCode-2167 移除所有载有违禁货物车厢所需的最少时间](https://leetcode.cn/problems/minimum-time-to-remove-all-cars-containing-illegal-goods/)

### Solution 1
很有趣的一道动态规划题.
需要移除字符串中的所有 $'1'$ , 由于移除操作的对称性, 在思考状态转移方程时, 我们不妨也从两个方向思考.
考虑字符串中的一个下标 $i$ , 要移除字符串中所有的 $'1'$ , 需要我们移除 $i$ 左侧的所有 $'1'$ 与 $i$ 右侧的所有 $'1'$ , 因此我们考虑这样的状态量: $left[i], right[i]$ 分别表示移除 $[0, i]$ 和 $[i, n - 1]$ 所需的最少步骤. 很明显最终答案等于 $min(left[i] + right[i + 1]), i=0, 1,...,n - 1$ (注意不要越界, 记 $right[n] = 0$).
接下来考虑状态转移方程. 当 $s[i] == '0'$, 有 $left[i] = left[i-1]$. 当 $s[i] == '1'$ 时, 我们有两种移除策略, 移除左侧所有 $'1'$ ,再单独移除 $s[i]$, 或者直接把前 $i + 1$ 个字符全部移除, 故有状态转移方程 $left[i] = min(left[i - 1] + 2, i + 1)$. 对于 $right$ 的处理类似.
代码如下:
```C++
class Solution {
public:
    int minimumTime(string s) {
        int n = s.size();
        vector<int> left(n + 1, 0);
        vector<int> right(n + 1, 0);
        if (s[0] == '1') {
            left[0] = 1;
        }
        for (int i = 1; i < n; i++) {
            if (s[i] == '0') {
                left[i] = left[i - 1];
            }
            else {
                left[i] = min(left[i - 1] + 2, i + 1);
            }
        }
        if (s[n - 1] == '1') {
            right[n - 1] = 1;
        }
        for (int i = n - 2; i >= 0; i--) {
            if (s[i] == '0') {
                right[i] = right[i + 1];
            }
            else {
                right[i] = min(right[i + 1] + 2, n - i);
            }
        }
        int ans = 1919810;
        for (int i = 0; i < n; i++) {
            ans = min(ans, left[i] + right[i + 1]);
        }
        return ans;
    }
};
```