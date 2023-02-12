---
title: CodeForces-1467C Three Bags 
date: 2022-07-26 16:54:00
updated:
tags: [计算机, 算法, CodeForces, C++, 贪心, 构造]
categories: CodeForces题解
cover: https://raw.githubusercontent.com/wsylab/wsyPicGo/main/img/photo-1601987078664-863b07dc0907
description: CodeForces-1467C「Three Bags」的思考与解答
---
### [CodeForces-1467C Three Bags](http://codeforces.com/problemset/problem/1467/C)
### 题目大意
输入三个长度分别为 $n_1, n_2, n_3$ 的数组 $a, b, c$ . 每次操作选两个数组各取出一个数, 作差后再放回其中一个数组中. 经过 $n_1 + n_2 + n_3$ 次操作后只剩下一个数, 求可能的最大值.
### Solution 1
对于三个数 $x, y, z$ , 可以把 $y$ 作为跳板, 通过 $x - (y - z)$ 来把 $z$ 加到 $x$ 上. 下面来选择承担 $y$ 功能的数. 如果想把数加到 $a_i$ 上, 对于 $a_j, j\not = i$ , 可以用 $b, c$ 中的数减去. 对于 $b, c$ 中的数, 这里有两种决策: 可以各选择对方的一个数作为跳板, 以及选择一整个数组作为另一个数组跳板, 这取决于是否存在一个整体都很大的数组. 
对于前一种决策, 需要从三个数组的最小值里选出两个作为跳板, 最终结果为 
$$\sum_{1\leq i\leq n_1}a_i + \sum_{1\leq i\leq n_2}b_i + \sum_{1\leq i\leq n_3}c_i - 2\times (\underset{1\leq i\leq n_1}{min}a_i + \underset{1\leq i\leq n_2}{min}b_i + \underset{1\leq i\leq n_3}{min}c_i - \underset{S = a,b,c}{max}\underset{x\in S}{min}\ x)
$$
对于后一种决策, 最终结果为 
$$\sum_{1\leq i\leq n_1}a_i + \sum_{1\leq i\leq n_2}b_i + \sum_{1\leq i\leq n_3}c_i - 2\times \underset{S=a,b,c}{min}\sum_{x\in S}x
$$
答案是两者的最大值.
代码如下:
```C++
#include "bits/stdc++.h"
using namespace std;
#define ll long long
int main() {
    ll n1, n2, n3;
    ll m1 = 1e9 + 7, m2 = 1e9 + 7, m3 = 1e9 + 7;
    ll s1 = 0, s2 = 0, s3 = 0;
    ll ans = 0;
    ll t;
    cin>>n1>>n2>>n3;
    while (n1--) {
        cin>>t;
        m1 = min(m1, t);
        s1 += t;
    }
    while (n2--) {
        cin>>t;
        m2 = min(m2, t);
        s2 += t;
    }
    while (n3--) {
        cin>>t;
        m3 = min(m3, t);
        s3 += t;
    }
    ans = s1 + s2 + s3 - 2 * (m1 + m2 + m3 - max(max(m1, m2), m3));
    ans = max(ans, s1 + s2 - s3);
    ans = max(ans, s1 + s3 - s2);
    ans = max(ans, s2 + s3 - s1);
    cout<<ans<<endl;
    return 0;
}
```