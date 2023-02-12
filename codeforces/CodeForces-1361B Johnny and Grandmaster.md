---
title: CodeForces-1361B Johnny and Grandmaster 
date: 2023-02-10 16:16:00
updated:
tags: [计算机, 算法, CodeForces, C++]
categories: CodeForces题解
cover: 
description: CodeForces-1361B「Johnny and Grandmaster」的思考与解答
---
### [CodeForces-1361B Johnny and Grandmaster](https://codeforces.com/problemset/problem/1361/B)
### 题目大意
给定 $p$ 和长度为 $n$ 的数组 $k$ . 把数组 $k$ 划分成两部分 $A,B$, 求 $\vert \sum_{v\in A}p^{v} - \sum_{v\in B}p^{v}\vert$ 的最小值.
### Solution 1
由于幂次的特性, 不难想到贪心. 取最大元素的放入一个子数组中, 然后取稍小的元素尽可能将其抵消. 比较 tricky 的部分是用栈 + 拆分实现这个过程. 
代码如下:
```C++
#include <bits/stdc++.h>
using namespace std;

#define ll long long

const ll MOD = 1e9 + 7;

ll mypow(ll p, ll k) {
    ll res = 1;
    while (k) {
        if (k & 1) {
            res = (res * p) % MOD;
            k--;
        }
        k /= 2;
        p = p * p % MOD;
    }
    return res;
}

int main() {
    int t;
    cin >> t;
    while (t--) {
        ll ans = 0;
        ll n, p;
        cin >> n >> p;
        vector<ll> a(n);
        for (int i = 0; i < n; i++) {
            cin >> a[i];
        }
        if (p == 1) {
            cout << (n & 1) << endl;
            continue;
        }
        sort(a.begin(), a.end(), greater<ll>());
        stack<int> s;
        s = stack<int>();
        int i = 0;
        for (i = 0; i < n; i++) {
            if (s.empty()) {
                s.push(a[i]);
            } else {
                if (a[i] < s.top()) { 
                    bool isEnd = false;
                    ll cnt = 1;
                    for (int j = 0; j < s.top() - a[i] && !isEnd; j++) {
                        cnt *= p;
                        if (cnt >= n - i) {
                            isEnd = true; 
                            break;
                        }
                    }
                    if (isEnd) {
                        break;
                    } 
                    else {
                        s.pop();
                        while (cnt--) {
                            s.push(a[i]);
                        }
                    }
                }
                if (a[i] == s.top()) {
                    s.pop();
                }
            }
        }
        while (!s.empty()) {
            ans = (ans + mypow(p, s.top())) % MOD;
            s.pop();
        }
        for (int k = i; k < n; k++) {
            ans = (ans - mypow(p, a[k]) + MOD) % MOD;
        }
        cout << ans << endl;
    }
    return 0;
}
```