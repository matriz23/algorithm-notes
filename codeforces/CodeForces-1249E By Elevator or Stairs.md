---
title: CodeForces-1249E By Elevator or Stairs? 
date: 2022-08-26 10:07:00
updated:
tags: [计算机, 算法, CodeForces, C++, 动态规划]
categories: CodeForces题解
cover: https://raw.githubusercontent.com/wsylab/wsyPicGo/main/img/photo-1618861818935-92632b6b7c4a.avif
description: CodeForces-1249E「By Elevator or Stairs?」的思考与解答
---
### [CodeForces-1249E By Elevator or Stairs?](https://codeforces.com/problemset/problem/1249/E)
### 题目大意
有 $n$ 层楼, 从 $i$ 层楼到第 $i + 1$ 层楼可以走楼梯, 花费 $a_i$ 时间, 也可以坐电梯, 花费 $b_i$ 时间, 但是作一段路程的电梯需要固定时间费用 $c$ . 返回到 $1, 2, ..., n$ 各自需要的最短时间. 
### Solution 1
第 $i$ 层楼到第 $i + 1$ 层楼, 如果选择走楼梯, 花费一定是 $a_i$ ; 如果选择坐电梯, 花费则与到达 $i$ 层楼的方式有关. 因此我们用 $dp[i][j]$ 代表到达 $i$ 层楼, 完成最后一段路程的方式为 $j$ 所需要的最少时间, 则状态转移方程如下:
$$
\begin{cases}
dp[i][0] = min(dp[i - 1][0], dp[i - 1][1]) + a[i - 1]\\
dp[i][1] = min(dp[i - 1][0] + b[i - 1] + c, dp[i - 1][1] + b[i - 1])
\end{cases}
$$
对于第 $i$ 层楼, $min(dp[i][0], dp[i][1])$ 就是所需的最少时间. 实际编写代码时可以用滚动数组优化.
代码如下:
```C++
#include <bits/stdc++.h>
using namespace std;
#define ll long long
const ll MOD = 1e9 + 7;
int main() {
    int n, c;
    cin>>n>>c;
    vector<int> a(n, 0), b(n, 0);
    for (int i = 1; i < n; i++) {
        cin>>a[i];
    }
    for (int i = 1; i < n; i++) {
        cin>>b[i];
    }
    int l_a = 0, l_b = 0, n_a = 0, n_b = 0;
    for (int i = 0; i < n; i++) {
        n_a = (i == 0)? 0: min(l_a + a[i], l_b + a[i]);
        n_b = (i == 0)? c: min(l_a + b[i] + c, l_b + b[i]);
        cout<<min(n_a, n_b)<<" ";
        l_a = n_a;
        l_b = n_b;
    }
    return 0;
}
```