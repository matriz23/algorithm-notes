---
title: CodeForces-1527C Sequence Pair Weight 
date: 2022-07-27 13:31:00
updated:
tags: [计算机, 算法, CodeForces, C++]
categories: CodeForces题解
cover: https://raw.githubusercontent.com/wsylab/wsyPicGo/main/img/photo-1497048502537-9e5719098e2b
description: CodeForces-1527C「Sequence Pair Weight」的思考与解答
---
### [CodeForces-1527C Sequence Pair Weight](https://codeforces.com/problemset/problem/1527/C)
### 题目大意
给定一个长度为 $n$ 的序列 $a$, 求
$$
\sum_{1\le l<r\le n}\sum_{l\le i<j\le r}I(a_i = a_j)
$$
### Solution 1
求 $a$ 子数组相等元素的对数和, 考虑每一对元素的贡献, 对于 $a_i = a_j$ , 在 $i\times (n + 1 - j)$ 个子数组中出现过. 我们使用哈希表存储一个值对应的下标, 利用后缀和优化即可.
代码如下:
```C++
#include "bits/stdc++.h"
using namespace std;
#define ll long long
// 前缀/后缀和优化
int main() {
    int t;
    cin>>t;
    while (t--) {
        int n;
        cin>>n;
        ll ans = 0;
        vector<ll> a(n, 0);
        map<ll, vector<ll>> book;
        for (int i = 0; i < n; i++) {
            cin>>a[i];
            book[a[i]].push_back(i);
        }
        for (auto it: book) {
            int m = it.second.size();
            vector<ll> suf(m + 1, 0);
            for (int i = m - 1; i >= 0; i--) {
                suf[i] = suf[i + 1] + n - it.second[i];
            }
            for (int i = 0; i < m; i++) {
                ans += (it.second[i] + 1) * suf[i + 1];
            }
        }
        cout<<ans<<endl;
    }
    return 0;
}
```
