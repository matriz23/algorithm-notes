---
title: CodeForces-66D Petya and His Friends 
date: 2022-08-15 17:57:00
updated:
tags: [计算机, CodeForces, C++, 数学]
categories: CodeForces题解
cover: https://raw.githubusercontent.com/wsylab/wsyPicGo/main/img/photo-1489769697626-b604955909a0
description: CodeForces-66D「Petya and His Friends」的思考与解答
---
### [CodeForces-66D Petya and His Friends]()
### 题目大意
给定 $n$ $(2\leq n\leq 50)$, 构造 $a_1, a_2,..., a_n$ 使得:
- $gcd(a_i, a_j) \not = 1, \forall\ 1\leq i, j\leq n$
- $gcd(a_1, a_2, ..., a_n) = 1$
- $a_i\not = a_j, \forall\ 1\leq i\not = j\leq n$

如果不存在合法构造, 输出 $-1$ .

### Solution 1
$n = 2$ 时, 前两条要求矛盾, 输出 $-1$ . 
$n \geq 3$ 时, 满足题意的构造均存在. 考虑 $a_1, a_2, ..., a_n$ 的因子 (不妨用 Venn 图来理解) , 用 $2, 3, 5$ 作为基本因子, 任意一个元素包含其中两个即可. 一个简单的构造如下: $2\times 3, 2\times 5, 3\times 5, 2\times 3\times 5, 2^2\times 3\times 5, ..., 2^{n - 3}\times 3\times 5$ .
代码如下:
```C++
#include <bits/stdc++.h>
using namespace std;
#define ll long long

int main() {
    int n;
    cin>>n;
    if (n == 2) {
        cout<<-1<<endl;
        return 0;
    }
    cout<<6<<endl<<10<<endl<<15<<endl;
    ll res = 15;
    for (int i = 3; i < n; i++) {
        res *= 2;
        cout<<res<<endl;
    }
    return 0;
}
```