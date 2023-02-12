---
title: CodeForces-1305C Kuroni and Impossible Calculation 
date: 2022-07-26 15:55:00
updated:
tags: [计算机, 算法, CodeForces, C++, 数学]
categories: CodeForces题解
cover: https://raw.githubusercontent.com/wsylab/wsyPicGo/main/img/photo-1518099074609-c9292e0bbc6c
description: CodeForces-1305C「Kuroni and Impossible Calculation」的思考与解答
---
### [CodeForces-1305C Kuroni and Impossible Calculation]()
### 题目大意
给定正整数 $m$ 和一个长度为 $n$ 的数组 $a_1, a_2, ..., a_n$ , 求 $\prod_{1\leq i < j\leq n}\vert a_i - a_j\vert\ mod\ m$ .
### Solution 1
Trick 题. 本题的模数 $m$ 范围相对较小. 对于 $n > m$ 的情况, 由抽屉原理, $\exist \ 1\leq i < j\leq n, s.t.\ a_i\equiv a_j\ mod\ m$ , 故 $\prod_{1\leq i < j\leq n}\vert a_i - a_j\vert \equiv 0\ mod\ m$ . 对于 $n \leq m$ 的情况, 数据范围较小, 直接暴力求解.
代码如下:
```C++
#include "bits/stdc++.h"
using namespace std;
#define ll long long
int main() {
    ll n, m;
    cin>>n>>m;
    vector<ll> a(n, 0);
    for (int i = 0; i < n; i++) {
        cin>>a[i];
    }
    if (n > m) {
        cout<<0<<endl;
    }
    else {
        sort(a.begin(), a.end());
        ll ans = 1;
        for (int i = 0; i < n; i++) {
            for (int j = i + 1; j < n; j++) {
                ans = (ans * (a[j] - a[i])) % m;
            }
        }
        cout<<ans<<endl;
    }
    return 0;
}
```