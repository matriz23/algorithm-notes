---
title: CodeForces-1582E Pchelyonok and Segments 
date: 2022-08-24 14:03:00
updated:
tags: [计算机, 算法, CodeForces, C++, 动态规划]
categories: CodeForces题解
cover: https://raw.githubusercontent.com/wsylab/wsyPicGo/main/img/photo-1490750967868-88aa4486c946.avif
description: CodeForces-1582E「Pchelyonok and Segments」的思考与解答
---
### [CodeForces-1582E Pchelyonok and Segments](http://codeforces.com/problemset/problem/1582/E)
### 题目大意
给定长度为 $n$ 的数组 $a$ , 要求从 $a$ 中选取尽可能多的、互不相交的子数组, 记这些子数组数量一共有 $k$ 个, 它们需要满足:
- 从左到右, 子数组长度分别为 $k, k - 1, ..., 1$ ;
- 从左到右, 子数组的元素和递增.

求 $k$ 的最大值.
### Solution 1
从左向右选, 第一个子数组的长度 $k$ 是不确定的, 不太方便. 不妨把 $a$ 反转, 这样要求变成了从左向右取, 长度递增, 元素和递减. 
一个朴素的想法是, 每次取数组时, 在满足其它要求的情况下尽可能取元素和较大的, 这会使得下一步取数组变得简单一些. 基于这种想法, 我们考虑使用动态规划进行处理取数过程中的和. 记 $dp[i][j]$ 为前 $i + 1$ 个元素, 取了 $j$ 个子数组时最后一个数组和的最大值. 对于 $dp[i][j]$ , 它可以直接选择不取, 从 $dp[i - 1][j]$ 转化而来; 也可以选择取一个长度为 $j$ 的子数组, 从 $dp[i - j][j - 1]$ 转化而来. 状态转移方程如下:
$$
dp[i][j] = 
\begin{cases}
max(dp[i - 1][j], dp[i - j][j - 1]),\ if\ \  \sum_{k = i - j + 1}^{i}a_i < dp[i - j][j - 1]\\
dp[i - 1][j] ,\ otherwise
\end{cases}
$$
最终, 寻找最大的 $j$ 使得 $dp[n - 1][j]$ 不为 $0$ , 这个 $j$ 就是我们求的 $k$ 的最大值.
代码如下:
```C++
#include <bits/stdc++.h>
using namespace std;
#define ll long long
const ll MOD = 1e9 + 7;
int main() {
    int t;
    cin>>t;
    while (t--) {
        int n;
        cin>>n;
        int m = sqrt(2 * n);
        vector<ll> a(n, 0), pre(n, 0);
        for (int i = 0; i < n; i++) {
            cin>>a[n - 1 - i];
        }
        for (int i = 0; i < n; i++) {
            pre[i] = (i == 0)? a[i]: a[i] + pre[i - 1];
        }
        vector<vector<ll>> dp(n, vector<ll>(m + 1, 0));
        for (int i = 0; i < n; i++) {
            dp[i][0] = 1e14;
        }
        dp[0][1] = a[0];
        for (int i = 1; i < n; i++) {
            for (int j = 1; j <= min(i, m); j++) {
                dp[i][j] = max(dp[i][j], dp[i - 1][j]);
                ll sum = pre[i] - pre[i - j];
                if (sum < dp[i - j][j - 1]) {
                    dp[i][j] = max(dp[i][j], sum);
                }
            }
        }
        for (int j = m; j >= 0; j--) {
            if (dp[n - 1][j] != 0) {
                cout<<j<<endl;
                break;
            }
        }
    }
    return 0;
}
```