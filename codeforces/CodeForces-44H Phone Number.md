---
title: CodeForces-44H Phone Number 
date: 2022-07-20 15:25:00
updated:
tags: [计算机, 算法, CodeForces, C++, 动态规划]
categories: CodeForces题解
cover: https://raw.githubusercontent.com/wsylab/wsyPicGo/main/img/1928e537
description: CodeForces-44H「Phone Number」的思考与解答
---
### [CodeForces-44H Phone Number](https://codeforces.com/contest/44/problem/H)
### 题目大意
给定一个长度为 $n$ 的数列 $s$ , $0\leq s[i]\leq 9$ , 按如下方式生成长度同样为 $n$ 的数列 $t$ :
- $t[0] \in \{0, 1, ..., 9\}$
- 如果 $s[i] + t[i - 1]$ 能被 $2$ 整除, 则 $t[i] = \frac{s[i] + t[i - 1]}{2}$ ; 否则 $t[i] = \lfloor \frac{s[i] + t[i - 1]}{2}\rfloor$ 或 $\lceil \frac{s[i] + t[i - 1]}{2}\rceil$ . 

求生成的数列中, 与 $s$ 不相同的数列的个数.

### Solution 1
先考虑生成的 $t$ 的个数. 根据 $t[i - 1]$ 可以得到 $t[i]$ 的可能值, 每次生成带来两个维度的变化: 数列长度 $i$ 与末尾元素 $j$ . 我们记 $dp[i][j]$ 为生成过程中长度 $i$ 且末尾元素为 $j$ 的数列个数.
直接写出 $dp[i][j]$ 由哪些 $dp[i - 1][k]$ 转化而来有些困难, 我们遍历 $dp[i - 1][k]$ 来进行递推(俗称 "刷表法" ) , 按题意可以计算出 $dp[n][j]$ 的值, 对 $j$ 累加就得到了所有 $t$ 的个数. 
如果生成的数列 $t$ 与 $s$ 相同, 则对于 $2\leq i\leq n$ , 有 $s[i] = \lfloor \frac{s[i] + s[i - 1]}{2}\rfloor$ 或 $\lceil \frac{s[i] + s[i - 1]}{2}\rceil$ , 得到 $\vert s[i] - s[i - 1]\vert \leq 1$ . 根据这一点可以判断输出答案时是否需要 $-1$ .
代码如下:
```C++
#include <bits/stdc++.h>
using namespace std;
int main() {
    string s;
    cin>>s;
    int n = s.size();
    vector<long long> a(n + 1, 0);
    for (int i = 1; i <= n; i++) {
        a[i] = s[i - 1] - '0';
    }
    vector<vector<long long>> dp(n + 1, vector<long long>(10, 0));
    for (int j = 0; j <= 9; j++) {
        dp[1][j] = 1;
    }
    for (int i = 2; i <= n; i++) {
        for (int j = 0; j <= 9; j++) {
            if ((j + a[i]) % 2 == 0) {
                dp[i][(j + a[i]) / 2] += dp[i - 1][j];
            }
            else {
                dp[i][(j + a[i]) / 2] += dp[i - 1][j];
                dp[i][(j + a[i]) / 2 + 1] += dp[i - 1][j];
            }
        }
    }
    long long ans = 0;
    for (int j = 0; j <= 9; j++) {
        ans += dp[n][j];
    }
    for (int i = 2; i <= n; i++) {
        if (abs(a[i] - a[i - 1]) > 1) {
            cout<<ans<<endl;
            return 0;
        }
    }
    cout<<ans - 1<<endl;
    return 0;
}
```