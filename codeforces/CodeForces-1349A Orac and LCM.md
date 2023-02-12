---
title: CodeForces-1349A Orac and LCM
date: 2022-07-25 18:01:00
updated:
tags: [计算机, 算法, CodeForces, C++, 数学, 数论]
categories: CodeForces题解
cover: https://raw.githubusercontent.com/wsylab/wsyPicGo/main/img/photo-1445964047600-cdbdb873673d
description: CodeForces-1349A「Orac and LCM」的思考与解答
---
### [CodeForces-1349A Orac and LCM](http://codeforces.com/problemset/problem/1349/A)
### 题目大意
给定 $n$ 个正整数 $a_1, ..., a_n$ , 求 $gcd(\{lcm(a_i, a_j)|1\leq i< j\leq n\})$ .
### Solution 1
考虑 $gcd(\{lcm(a_i, a_j)|1\leq i< j\leq n\})$ 的含义. 对于每个质因子, 两两之间找出更大的指数, 取最小值, 即找出每个质因子第二小的指数. 
容易想到 $gcd(\{a_i|1\leq i\leq n\})$ 能够找出每个质因子最小的指数, 如何求出第二小的指数呢? 考虑 $a_i × a_j$ , 这一操作会叠加指数, 则计算 $gcd(\{a_ia_j|1\leq i<j\leq n\})$ 可以得到每个质因子最小的指数与第二小的指数之和, $\frac{gcd(\{a_ia_j|1\leq i<j\leq n\})}{gcd(\{a_i|1\leq i\leq n\})}$ 就是我们要找的第二小的指数. 因为 $gcd(\{a_ia_j|1\leq i<j\leq n\}) = gcd(\{gcd(\{a_ia_j|1\leq i< j\})\ |\ 1< j\leq n\}) =  gcd(\{a_j\times gcd(\{a_i|1\leq i< j\})\ |\ 1< j\leq n\})$ . 我们可以一边读取 $a_i$ 一边更新 $res$ 与 $gcd(\{a_j|1\leq j< i\})$ , 最后除以 $gcd(\{a_i|1\leq i\leq n\})$ 即可.
代码如下:
```C++
#include "bits/stdc++.h"
using namespace std;
#define ll long long
int main() {
    int n;
    cin>>n;
    vector<ll> a(n, 0);
    ll res = 1, gcd = 1;
    for (int i = 0; i < n; i++) {
        cin>>a[i];
        res = (i == 1) ? a[i] * gcd : __gcd(res, a[i] * gcd);
        gcd = (i == 0)? a[0]: __gcd(gcd, a[i]);
    }
    cout << res / gcd << endl;
    return 0;
}
```


