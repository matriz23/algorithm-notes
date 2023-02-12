---
title: CodeForces-467C George and Job 
date: 2022-08-24 10:46:00
updated:
tags: [计算机, 算法, CodeForces, C++, 动态规划]
categories: CodeForces题解
cover: https://raw.githubusercontent.com/wsylab/wsyPicGo/main/img/ZF75d1lhQ0SyLWYEGOqO_21_forest_mountains.jpg
description: CodeForces-467C「George and Job」的思考与解答
---
### [CodeForces-467C George and Job](https://codeforces.com/problemset/problem/467/C)
### 题目大意
给定 $n, m, k$ 和一个长度为 $n$ 的数组 $p$ . 从 $p$ 中选择 $m$ 个互不相交的连续子数组, 使得这些子数组的和最大, 求这个最大值.
### Solution 1
从 $n$ 个数中, 选出 $k$ 个长度为 $m$ 的连续子数组, 考虑用动态规划模拟这个选择的过程. 记 $dp[i][j]$ 为前 $i + 1$ 个元素中, 选出了 $j$ 个长度为 $m$ 的连续子数组所得到的最大和. 思考 $dp[i][j]$ 由什么状态转移过来  (也就是这一步能做什么) ? 对于 $p[i]$ , 我们要么以它为终点, 选出一个长度为 $m$ 的子数组; 要么这一步什么也不做. 因此状态转移方程如下:
$$
dp[i][j] = max(dp[i - m][j - 1] + \sum_{t = i - m + 1}^{i}p[t], dp[i - 1][j])
$$
最终 $dp[n - 1][k]$ 就是要求的答案.
代码如下:
```C++
#include <bits/stdc++.h>
using namespace std;
#define ll long long
const ll MOD = 1e9 + 7;
int main() {
    int n, m, k;
    cin>>n>>m>>k;
    vector<ll> p(n, 0);
    for (int i = 0; i < n; i++) {
        cin>>p[i];
    }
    ll sum = 0;
    for (int i = 0; i < m - 1; i++) {
        sum += p[i];
    }
    vector<vector<ll>> dp(n, vector<ll>(k + 1, 0));
    for (int i = m - 1; i < n; i++) {
        sum += (i - m < 0)? p[i]: p[i] - p[i - m];
        for (int j = 0; j <= min((i + 1) / m, k); j++) {
            dp[i][j] = max(dp[i][j], (i - 1 < 0)? 0: dp[i - 1][j]);
            if (j != 0) {
                dp[i][j] = max(dp[i][j], (i - m < 0)? sum: dp[i - m][j - 1] + sum);
            }
        }
    }
    cout<<dp[n - 1][k]<<endl;
    return 0;
}
```