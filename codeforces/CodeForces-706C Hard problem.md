---
title: CodeForces-706C Hard problem 
date: 2022-08-01 11:29:00
updated:
tags: [计算机, 算法, CodeForces, C++, 动态规划]
categories: CodeForces题解
cover: https://raw.githubusercontent.com/wsylab/wsyPicGo/main/img/photo-1635354952327-2820b80902c4
description: CodeForces-706C「Hard problem」的思考与解答
---
### [CodeForces-706C Hard problem](https://codeforces.com/problemset/problem/706/C)
### 题目大意
给定 $n$ 个字符串 $s_1, s_2, ..., s_n$ 和反转它们需要的代价 $a_1, a_2, ..., a_n$ . 返回把字符串数组修改成按字典序排列的最少代价; 如果不能做到这一点, 返回 $-1$ .
### Solution 1
对于一个字符串, 我们可以反转, 也可以选择不反转, 根据前一个字符串是否反转, 可以得到相应的代价变化. 
代码如下:
```C++
#include <bits/stdc++.h>
using namespace std;
#define ll long long
const ll MAXN = 1e15 + 10;
string reverse_string(string s) {
    string res = "";
    for (int i = s.size() - 1; i >= 0; i--) {
        res += s[i];
    }
    return res;
}

int main() {
    int n;
    cin>>n;
    vector<ll> a(n, 0);
    vector<string> s(n), r(n);
    for (int i = 0; i < n; i++) {
        cin>>a[i];
    }
    for (int i = 0; i < n; i++) {
        cin>>s[i];
        r[i] = reverse_string(s[i]);
    }
    vector<vector<ll>> dp(n, vector<ll>(2, MAXN));
    // dp[i][0] 表示 0 - i 个字符串, 第 i 个不反转 合法的最小代价
    dp[0][0] = 0;
    dp[0][1] = a[0];
    for (int i = 1; i < n; i++) {
        if (s[i] >= s[i - 1]) {
            dp[i][0] = min(dp[i][0], dp[i - 1][0]);
        }
        if (s[i] >= r[i - 1]) {
            dp[i][0] = min(dp[i][0], dp[i - 1][1]);
        }
        if (r[i] >= s[i - 1]) {
            dp[i][1] = min(dp[i][1], dp[i - 1][0] + a[i]);
        }
        if (r[i] >= r[i - 1]) {
            dp[i][1] = min(dp[i][1], dp[i - 1][1] + a[i]);
        }
    }
    ll ans = min(dp[n- 1][0], dp[n - 1][1]) == MAXN? -1: min(dp[n- 1][0], dp[n - 1][1]);
    cout<<ans<<endl;
    return 0;
}

```