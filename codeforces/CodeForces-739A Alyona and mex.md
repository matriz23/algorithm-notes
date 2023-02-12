---
title: CodeForces-739A Alyona and mex 
date: 2022-07-18 10:37:00
updated:
tags: [计算机, 算法, CodeForces, C++, 构造]
categories: CodeForces题解
cover: https://raw.githubusercontent.com/wsylab/wsyPicGo/main/img/photo-1510951366214-f8ce654864ac
description: CodeForces-739A「Alyona and mex」的思考与解答
---
### [CodeForces-739A Alyona and mex](https://codeforces.com/problemset/problem/739/A)
### 题目大意
定义 $mex(S)$ 为 $S$ 中未出现的最小非负整数. 给定 $n$ 和一系列区间 $[l_i, r_i], 1\leq l_i\leq r_i\leq n$ , 构造一个长度为 $n$ 的数列 $a$ , 使得 $\underset{1\leq i\leq m}{min}\ mex(a[l_i, r_i])$ 最大.
### Solution 1
很精彩的一道构造题!
单独考虑一个区间 $[l_i, r_i]$ , $mex((a[l_i, r_i]) = r_i - l_i + 1$ , 我们把区间填满 $0, 1, ..., r_i - l_i$ 即可. 考虑多个区间的情形是这道题的关键. 一个很容易陷入的误区是过于考虑区间交叠的细节处理. 如果试图从各个区间的重叠情况来进行贪心处理, 很难有一个令人满意的策略. 现在已经知道 $\underset{1\leq i\leq m}{min}\ mex(a[l_i, r_i])$ 的一个上界 $L = \underset{1\leq i\leq m}{min}\ r_i - l_i + 1$ , 不如关心能否给出一个构造使得上界能够取到. 要使得任意长度 $\geq L$ 的区间都覆盖了 $0, 1, ..., L - 1$ ,  一个合题意的构造是 $0, 1, ..., L - 1, 0, 1, ....$ .
代码如下: 
```C++
#include <bits/stdc++.h>
using namespace std;
int main() {
    int n, m;
    cin>>n>>m;
    int len = INT_MAX;
    for (int i = 0; i < m; i++) {
        int l, r;
        cin>>l>>r;
        len = min(r - l + 1, len);
    }
    cout<<len<<endl;
    for (int i = 0; i < n; i++) {
        cout<<i % len<<" ";
    }
    cout<<endl;
    return 0;
}
```