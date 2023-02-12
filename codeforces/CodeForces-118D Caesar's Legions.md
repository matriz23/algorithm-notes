---
title: CodeForces-118D Caesar's Legions 
date: 2022-07-25 16:33:00
updated:
tags: [计算机, 算法, CodeForces, C++]
categories: CodeForces题解
cover: https://raw.githubusercontent.com/wsylab/wsyPicGo/main/img/photo-1580136579312-94651dfd596d
description: CodeForces-118D「Caesar's Legions」的思考与解答
---
### [CodeForces-118D Caesar's Legions](https://codeforces.com/problemset/problem/118/D)
### 题目大意
给定 $0$ 的个数 $n_0$ , $1$ 的个数 $n_1$ , 以及 $k_0, k_1$ , 把 $0\ 1$ 排成一列, 要求连续的 $0$ 个数不超过 $k_0$ , 连续的 $1$ 个数不超过 $k_1$ , 求满足要求的排列的个数.
### Solution 1
考虑一个合法的排列, 为了方便递推, 我们关心的是排列长度、末尾元素的种类以及个数. 为了保证排列是由给定的元素构成的, 还要加上一维来代表剩余 $0$ 的个数. 我们用 $dp[i][type][j][r]$ 代表长度为 $i$ , 末尾元素种类为 $type$ , 末尾连续元素个数为 $j$ , 还剩余 $r$ 个 $0$ 的排列个数. 对于 $j \geq 2$ 的排列, 需要从 $j - 1$ 转移得到; 对于 $j = 1$ 的排列, 需要从另一种元素结尾的排列转移得到. 状态转移方程如下:
$$
dp[i][type][j][r] = 
\begin{cases}
\sum_{1\leq p\leq k_{1 - type}}dp[i - 1][1 - type][p][r + I(type = 1)], j = 1\\
dp[i - 1][type][j - 1][r + I(type = 0)], j\geq 2
\end{cases}
$$
代码如下:
```C++
#include "bits/stdc++.h"
using namespace std;
#define ll long long
const int MOD = 1e8;
int main() {
    int n[2], k[2];
    cin>>n[0]>>n[1]>>k[0]>>k[1];
    int N = n[0] + n[1];
    int dp[210][2][11][110];
    dp[1][0][1][n[0] - 1] = 1;
    dp[1][1][1][n[0]] = 1;
    for (int i = 2; i <= N; i++) {
        // type == 0
        for (int j = 1; j <= min(n[0], k[0]); j++) {
            for (int r = 0; r <= n[0]; r++) {
                if (j != 1) {
                    dp[i][0][j][r] = dp[i - 1][0][j - 1][r + 1];
                }
                else {
                    for (int p = 1; p <= min(n[1], k[1]); p++) {
                        dp[i][0][j][r] = (dp[i][0][j][r] + dp[i - 1][1][p][r + 1]) % MOD;
                    }
                }
            }
        }
        // type == 1
        for (int j = 1; j <= min(n[1], k[1]); j++) {
            for (int r = 0; r <= n[0]; r++) {
                if (j != 1) {
                    dp[i][1][j][r] = dp[i - 1][1][j - 1][r];
                }
                else {
                    for (int p = 1; p <= min(n[0], k[0]); p++) {
                        dp[i][1][j][r] = (dp[i][1][j][r] + dp[i - 1][0][p][r]) % MOD;
                    }
                }
            }
        }
    }
    ll res = 0;
    for (int type = 0; type <= 1; type++) {
        for (int j = 1; j <= k[type]; j++) {
            res = (res + dp[N][type][j][0]) % MOD;
        }
    }
    cout<<res<<endl;
    return 0;
}
```