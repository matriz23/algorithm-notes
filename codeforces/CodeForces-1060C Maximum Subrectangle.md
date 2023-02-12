---
title: CodeForces-1060C Maximum Subrectangle 
date: 2022-08-03 19:34:00
updated:
tags: [计算机, 算法, CodeForces, C++]
categories: CodeForces题解
cover: https://raw.githubusercontent.com/wsylab/wsyPicGo/main/img/photo-1561470872-2e4b48435150.jpg
description: CodeForces-1060C「Maximum Subrectangle」的思考与解答
---
### [CodeForces-1060C Maximum Subrectangle](http://codeforces.com/problemset/problem/1060/C)
### 题目大意
给定长度为 $n$ 的数列 $a$ 和长度为 $m$ 的数列 $b$ , 根据 $a, b$ 构造出一个 $n \times m$ 的矩阵 $c$ , 满足 $c_{ij} = a_i\times b_j$ . 给出一个上界 $x$ , 求 $c$ 的所有子矩阵中, 元素和不超过 $x$ 的子矩阵的最大面积.

### Solution 1
子矩阵面积 $\sum_{i = x_1}^{x_2}\sum_{j = y_1}^{y_2}c_{ij} = \sum_{i = x_1}^{x_2}a_i\times \sum_{j = y_1}^{y_2}b_j$ , 实际上是寻找 $a, b$ 的子段和, 在满足子段和积 $\leq x$ 的条件下寻找子段长度积的最大值. 因此, 我们希望对于一个子段, 在值尽可能小的情况下, 长度尽可能大. 为了遍历所有情况, 我们固定长度, 记录 $a, b$ 每个长度子段的最小值, 再两两匹配, 如果满足条件再更新最大积. 
代码如下:
```C++
#include <bits/stdc++.h>
using namespace std;
#define ll long long
int main() {
    int n, m;
    cin>>n>>m;
    vector<ll> a(n, 0), b(m, 0);
    for (int i = 0; i < n; i++) {
        cin>>a[i];
        a[i] += (i == 0)? 0: a[i - 1];
    }
    for (int i = 0; i < m; i++) {
        cin>>b[i];
        b[i] += (i == 0)? 0: b[i - 1];
    }
    ll x;
    cin>>x;
    vector<ll> len_a(n + 1, 1e9 + 7), len_b(m + 1, 1e9 + 7);
    for (int len = 1; len <= n; len++) {
        for (int i = len - 1; i < n; i++) {
            ll seq_sum = (i == len - 1)? a[i]: a[i] - a[i - len];
            len_a[len] = min(len_a[len], seq_sum);
        }
    }
    for (int len = 1; len <= m; len++) {
        for (int i = len - 1; i < m; i++) {
            ll seq_sum = (i == len - 1)? b[i]: b[i] - b[i - len];
            len_b[len] = min(len_b[len], seq_sum);
        }
    }
    int ans = 0;
    for (int i = 1; i <= n; i++) {
        for (int j = m; j >= 0; j--) {
            if (len_a[i] * len_b[j] <= x) {
                ans = max(ans, i * j);
                break;
            }
        }
    }
    cout<<ans<<endl;
    return 0;
}
```

> 记录这道题目, 主要目的是学习一种枚举的思想: 对于约束很多的优化问题, 可以固定一个约束, 考虑另一个约束的最优值. 这样在遍历时可以节约很多时间.