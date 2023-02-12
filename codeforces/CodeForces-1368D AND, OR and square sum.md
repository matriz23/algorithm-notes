---
title: CodeForces-1368D AND, OR and square sum 
date: 2022-08-24 21:03:00
updated:
tags: [计算机, 算法, CodeForces, C++, 贪心]
categories: CodeForces题解
cover: https://raw.githubusercontent.com/wsylab/wsyPicGo/main/img/photo-1629706168156-d2d0558c972e.avif
description: CodeForces-1368D「AND, OR and square sum」的思考与解答
---
### [CodeForces-1368D AND, OR and square sum](https://codeforces.com/problemset/problem/1368/D)
### 题目大意
给定长度为 $n$ 的数组 $a$ . 每次操作可以选择 $a_i, a_j$ , 令 $a_i = a_i \& a_j, a_j = a_i | a_j$ . 返回任意次操作后能够得到的 $\sum_{0\leq i< n} a_i^2$ 的最大值.
### Solution 1
注意到 $a_i + a_j = a_i \& a_j + a_i | a_j$ , 而后者的更不平均. 这意味着每次操作都会让两者的平方和不减. 事实上, 一次操作把 $a_i, a_j$ 二进制上的 $1$ 集中在了某个元素身上. 用类似冒泡排序的过程可以模拟一个得到目标数列的操作, 不过复杂度是 $O(n^2)$ , 不足以通过本题. 更合理的策略是直接遍历 $a$ , 存储每一位上共有多少个 $1$ , 再重新分配即可.
代码如下:
```C++
#include <bits/stdc++.h>
using namespace std;
#define ll long long
const ll MOD = 1e9 + 7;
int main() {
    int n;
    cin>>n;
    vector<ll> a(n, 0);
    for (int i = 0; i < n; i++) {
        cin>>a[i];
    }
    vector<int> cnt(20, 0);
    for (auto t: a) {
        for (int i = 0; i < 20; i++) {
            if (t & 1 << i) {
                cnt[i]++;
            }
        }
    }
    ll ans = 0;
    while (n--) {
        ll t = 0;
        for (int i = 0; i < 20; i++) {
            if (cnt[i] >= n + 1) {
                t += (1 << i);
            }
        }
        ans += t * t;
    }
    cout<<ans<<endl;
    return 0;
}
```