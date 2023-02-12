---
title: LeetCode-1414 和为 K 的最少斐波那契数字数目 
date: 2022-11-14 16:36:00
updated:
tags: [计算机, 算法, LeetCode, C++, 贪心]
categories: LeetCode题解
cover: 
description: LeetCode-1414「和为 K 的最少斐波那契数字数目」的思考与解答
---
### [LeetCode-1414 和为 K 的最少斐波那契数字数目](https://leetcode.cn/problems/find-the-minimum-number-of-fibonacci-numbers-whose-sum-is-k/)

### Solution 1
贪心, 每次选不超过当前剩余数的最大斐波那契数即可.
详细的证明可以参考官方题解 [和为 K 的最少斐波那契数字数目](https://leetcode.cn/problems/find-the-minimum-number-of-fibonacci-numbers-whose-sum-is-k/solution/he-wei-k-de-zui-shao-fei-bo-na-qi-shu-zi-shu-mu-by/) .
代码如下:
```C++
class Solution {
public:
    #define ll long long
    ll F[100] = {0};
    ll f(int i) {
        if (F[i] != 0) {
            return F[i];
        }
        if (i == 1 || i == 2) {
            F[i] = 1;
            return 1;
        }
        F[i] = f(i - 1) + f(i - 2);
        return F[i];
    }
    int findMinFibonacciNumbers(int k) {
        int ans = 0;
        while (k != 0) {
            int l = 0;
            int r = 100;
            while (l < r) {
                int m = l + (r - l) / 2;
                if (f(m) <= k) {
                    l = m + 1;
                }
                else {
                    r = m;
                }
            }
            ans++;
            k -= f(l - 1);
        }
        return ans;
    }
};

```