---
title: CodeForces-1105C Ayoub and Lost Array 
date: 2022-07-14 14:41:00
updated:
tags: [计算机, 算法, CodeForces, C++, 动态规划]
categories: CodeForces题解
cover: https://raw.githubusercontent.com/wsylab/wsyPicGo/main/img/photo-1461301214746-1e109215d6d3
description: CodeForces-1105C「Ayoub and Lost Array」的思考与解答
---
### [CodeForces-1105C Ayoub and Lost Array](https://codeforces.com/problemset/problem/1105/C)
### 题目大意
给定三个正整数 $n, l, r$ , 求满足如下条件的数组数目:
- 数组有 $n$ 个元素
- 数组的每个元素都在 $[l, r]$ 内
- 数组元素之和为 $3$ 的倍数
### Solution 1
一下子构造出整个数组可能有些困难, 我们不妨一个个向数组里添加数. 添加数带来的是长度与数组和 $mod\ 3$ 两方面的变化, 可以用这两个参数作为数组的状态. 以 $dp[i][j]$ 表示长度为 $i$ , 元素和 $mod\ 3$ 为 $j$ 的数组个数, 则有如下状态转移方程:
$$
dp[i][j] = \sum_{k = 0}^{2}dp[i - 1][k] × num[(j - k)\ mod\ 3]
$$
其中 $num$ 数组我们可以预先处理出来.
代码如下:
```C++
#include <bits/stdc++.h>
using namespace std;
const int MOD = 1e9 + 7;
int main() {
    long long n, l, r;
    cin>>n>>l>>r;
    vector<long long>num(3, 0);
    num[0] = r / 3 - (l - 1) / 3;
    num[1] = (r + 2) / 3 - (l + 1) / 3;
    num[2] = (r + 1) / 3 - l / 3;
    vector<vector<long long>> dp(n + 1, vector<long long>(3, 0));
    dp[0][0] = 1;
    for (int i = 1; i <= n; i++) {
        for (int j = 0; j < 3; j++) {
            dp[i][j] = dp[i - 1][j] * num[0] + dp[i - 1][(j + 2) % 3] * num[1] + dp[i - 1][(j + 1) % 3] * num[2];
            dp[i][j] %= MOD;
        }
    }
    cout<<dp[n][0]<<endl;
    return 0;
}
```