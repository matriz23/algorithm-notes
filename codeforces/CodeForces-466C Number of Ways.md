---
title: CodeForces-466C Number of Ways 
date: 2022-07-22 13:34:00
updated:
tags: [计算机, 算法, CodeForces, C++]
categories: CodeForces题解
cover: https://raw.githubusercontent.com/wsylab/wsyPicGo/main/img/photo-1483213097419-365e22f0f258
description: CodeForces-466C「Number of Ways」的思考与解答
---
### [CodeForces-466C Number of Ways](https://codeforces.com/problemset/problem/466/C)
### 题目大意
给定一个长度为 $n$ 的数组 $a_1, a_2, ..., a_n$ , 求数对 $(i, j)$ 的数量, 其中 $(i, j)$ 满足 $2\leq i\leq j\leq n - 1$ 且 $\sum_{k = 1}^{i - 1}a_k =\sum_{k = i}^{j}a_k = \sum_{k = j + 1}^{n} a_k$ .

### Solution 1
数组和 $sum$ 显然必须是 $3$ 的倍数. 如果满足了这一点, 我们先寻找满足 $\sum_{k = 1}^{i}a_k = \frac{sum}{3}$ 的 $i$ , 对于每个 $i$ , 需要统计满足 $i < j\leq n - 1$ 与 $\sum_{k = 1}^{j}a_k = \frac{2}{3}\times sum$ 的 $j$ 的个数. 随着 $i$ 增大, $j$ 的数量严格不增, 所以我们可以用栈来计数.
代码如下:
```C++
#include <bits/stdc++.h>
using namespace std;
#define ll long long
int main() {
    int n;
    cin>>n;
    vector<ll> a(n + 1, 0);
    long long sum = 0;
    for (int i = 1; i <= n; i++) {
        cin>>a[i];
        a[i] += a[i - 1]; // 前缀和
    }
    if (a[n] % 3 != 0) {
        cout<<0<<endl;
        return 0;
    }
    vector<int> idx;
    stack<int> st;
    for (int i = 1; i <= n - 1; i++) {
        if (a[i] == a[n] / 3) {
            idx.push_back(i); // 存储 i
        }
        if (a[n - i] == a[n] / 3 * 2) {
            st.push(n - i); // 存储 j, j 倒着存
        }
    }
    ll ans = 0;
    for (int i = 0; i < idx.size(); i++) {
        while (st.top() <= idx[i]) {
            st.pop();
            if (st.empty()) {
                cout<<ans<<endl;
                return 0;
            }
        }
        ans += st.size();
    }
    cout<<ans<<endl;
    return 0;
}
```