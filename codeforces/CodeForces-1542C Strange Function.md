---
title: CodeForces-1542C Strange Function 
date: 2022-08-05 11:13:00
updated:
tags: [计算机, 算法, CodeForces, C++, 数论]
categories: CodeForces题解
cover: https://raw.githubusercontent.com/wsylab/wsyPicGo/main/img/photo-1508229476473-4d2bbb146423
description: CodeForces-1542C「Strange Function」的思考与解答
---
### [CodeForces-1542C Strange Function](http://codeforces.com/problemset/problem/1542/C)
### 题目大意
记 $f(i)$ 为最小的正整数 $x$ , 满足 $x$ 不是 $i$ 的因子. 给定正整数 $n$ , 计算 $\sum_{i=1}^n f(i)$ .
### Solution 1
先分析函数性质, 如果 $f(i) = x$ , 意味着 $1, 2, ..., x - 1$ 都是 $i$ 的因子, 而 $x \nmid i$ , 即 $lcm(1, 2, ..., x - 1)\mid i$ , 且 $lcm(1, 2, ..., x)\nmid i$ . 因此, $1, 2, ..., n$ 中符合 $f(i) = x$ 的数恰有 $\lfloor \frac{n}{lcm(1, 2, ..., x - 1)}\rfloor - \lfloor \frac{n}{lcm(1, 2, ..., x)}\rfloor$ 个, 累加每个 $x$ 对于 $ans$ 的贡献即可.
代码如下:
```C++
#include <bits/stdc++.h>
using namespace std;
#define ll long long
const ll MOD = 1e9 + 7;
int main() {
    int t;
    cin>>t;
    while (t--) {
        ll n;
        cin>>n;
        ll ans = 0;
        ll lcm = 1;
        for (ll i = 2; i < n + 2 && lcm <= n; i++) {
            ll temp = lcm * i / __gcd(lcm, i);
            ans = (ans + i * (n / lcm - n / temp)) % MOD;
            lcm = temp;
        }
        cout<<ans<<endl;
    }
    return 0;
}
```