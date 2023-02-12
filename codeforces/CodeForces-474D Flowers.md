---
title: CodeForces-474D Flowers 
date: 2022-07-25 11:53:00
updated:
tags: [计算机, 算法, CodeForces, C++, 字符串, 动态规划]
categories: CodeForces题解
cover: https://raw.githubusercontent.com/wsylab/wsyPicGo/main/img/photo-1590119977523-5af0d80c559f
description: CodeForces-474D「Flowers」的思考与解答
---
### [CodeForces-474D Flowers](https://codeforces.com/problemset/problem/474/D)
### 题目大意
给定正整数 $k$. 称一个 $01$ 字符串是好的当且仅当其可以通过不断把**连续** $k$ 个 $1$ **全部改成** $0$ 来成为全 $0$ 字符串. 求长度在 $[l, r]$ 内的好 $01$ 字符串个数.

### Solution 1
长度为 $n$ 的好 $01$ 字符串可以由长度为 $n - 1$ 的好 $01$ 字符串补上一个 $0$ 构成, 也可以由长度为 $n - k$ 的好 $01$ 字符串补上连续 $k$ 个 $1$ 构成. 记 $dp[i]$ 为长度为 $i$ 的好 $01$ 字符串个数, 则 $dp[i] = dp[i - 1] + dp[i - k]$ . 每次询问 $\sum_{i = l}^{r} dp[i]$ , 可以用前缀和处理.
代码如下:
```C++
#include "bits/stdc++.h"
using namespace std;
const int N = 1e5 + 10;
const int MOD = 1e9 + 7;
int dp[N];
int main() {
    int t, k;
    cin>>t>>k;
    for (int i = 0; i < k; i++) {
        dp[i] = 1;
    }
    for (int i = k; i < N; i++) {
        dp[i] = (dp[i - k] + dp[i - 1]) % MOD;
    }
    for (int i = 1; i < N; i++) {
        dp[i] = (dp[i] + dp[i - 1]) % MOD;
    }
    while (t--) {
        int l, r;
        cin>>l>>r;
        cout<<(dp[r] - dp[l - 1] + MOD) % MOD<<endl;
    }
    return 0;
}
```