---
title: CodeForces-1458A Row GCD  
date: 2022-07-20 17:23:00
updated:
tags: [计算机, 算法, CodeForces, C++, 数论]
categories: CodeForces题解
cover: https://raw.githubusercontent.com/wsylab/wsyPicGo/main/img/photo-1508671318294-1afb20379417
description: CodeForces-1458A「Row GCD」的思考与解答
---
### [CodeForces-1458A Row GCD](https://codeforces.com/problemset/problem/1458/A)
### 题目大意
给定长度为 $n$ 的数列 $a$ 和长度为 $m$ 的数列 $b$ , 对每个 $1\leq j\leq m$ , 计算 $gcd(a_1 + b_j, a_2 + b_j, ..., a_n + b_j)$ .
### Solution 1
参照辗转相除法的思想, $gcd(a_1 + b_j, a_2 + b_j, ..., a_n + b_j) = gcd(a_1 + b_j, a_2 - a_1, ..., a_n - a_1)$ . 我们预处理出 $gcd(a_2 - a_1, ..., a_n - a_1)$ , 再和 $a_1 + b_j$ 求最大公约数.
$n = 1$ 时需要特判.
代码如下:
```C++
#include <bits/stdc++.h>
using namespace std;
int main() {
    int n, m;
    cin>>n>>m;
    vector<long long> a(n, 0), b(m, 0);
    for (int i = 0; i < n; i++) {
        cin>>a[i];
    }
    for (int i = 0; i < m; i++) {
        cin>>b[i];
    }
    if (n == 1) {
        for (int j = 0; j < m; j++) {
            cout<<a[0] + b[j]<<" ";
        }
        cout<<endl;
        return 0;
    }
    sort(a.begin(), a.end());
    long long c = (n >= 3)? __gcd(a[2] - a[0], a[1] - a[0]): a[1] - a[0];
    for (int i = 3; i < n; i++) {
        c = __gcd(a[i] - a[0], c);
    }
    for (int j = 0; j < m; j++) {
        cout<<__gcd(a[0] + b[j], c)<<" ";
    }
    cout<<endl;
    return 0;
}
```