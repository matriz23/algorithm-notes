---
title: LeetCode-1819 序列中不同最大公约数的数目 
date: 2023-01-14 14:24:00
updated:
tags: [计算机, 算法, LeetCode, C++, 数论]
categories: LeetCode题解
cover: 
description: LeetCode-1819「序列中不同最大公约数的数目」的思考与解答
---
### [LeetCode-1819 序列中不同最大公约数的数目](https://leetcode.cn/problems/number-of-different-subsequences-gcds/)

### Solution 1
对于 $x$ , 如果它是某个子序列 $(a_0, a_1, ..., a_k)$ 的最大公约数, 那么它也一定是 $(a_0, a_1, ..., a_k, a_{k + 1} = x * y)$ 的最大公约数. 因此, 要验证 $x$ 是否为原数组的某个子序列的最大公约数, 验证数组中 $x$ 的倍数的最大公约数是否等于 $x$ 即可.
代码如下:
```C++
class Solution {
public:
    int countDifferentSubsequenceGCDs(vector<int>& nums) {
        bool s[200010] = {false};
        for (int num: nums) {
            s[num] = true;
        }
        int ans = 0;
        for (int i = 1; i <= 2e5; i++) {
            int res = -1;
            for (int j = 1; j < (2e5 + 10)  / i; j++) {
                if (s[i * j]) {
                    res = res == -1? j: __gcd(res, j);
                }
                if (res == 1) {
                    ans++;
                    break;
                }
            }
        }
        return ans;
    }
};
```