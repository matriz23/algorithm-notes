---
title: CodeForces-1554B Cobb 
date: 2022-07-29 11:00:00
updated:
tags: [计算机, 算法, CodeForces, C++]
categories: CodeForces题解
cover: https://raw.githubusercontent.com/wsylab/wsyPicGo/main/img/photo-1658847133295-1693456b858c
description: CodeForces-1554B「Cobb」的思考与解答
---
### [CodeForces-1554B Cobb](http://codeforces.com/problemset/problem/1554/B)
### 题目大意
给定 $n, k\ (2\leq n\leq 10^5, 1\leq k\leq min(n, 100))$ 和一个长度为 $n$ 的数组 $a\ (0\leq a_i \leq n)$ , 求 $\underset{1\leq i < j\leq n}{max}i\times j - k\times (a_i | a_j)$ .

### Solution 1
学会使用必要条件进行优化. $(a_i | a_j) < 2n$ , 故 $ans > n\times (n - 1) - k\times 2n$ . 而对于 $j$ , $i\times j - k\times (a_i | a_j) \leq j\times (j - 1)$ , 只有 $j\times (j - 1) > ans$ 才可能优化答案. 
代码如下:
```C++
#include "bits/stdc++.h"
using namespace std;
#define ll long long
int main() {
    int t;
    cin>>t;
    while (t--) {
        ll n, k;
        cin>>n>>k;
        vector<ll> a(n + 1, 0);
        for (int i = 1; i <= n; i++) {
            cin>>a[i];
        }
        ll ans = n * (n - 1) - 2 * k * n;
        for (ll j = n; j >= 2; j--) {
            if (j * (j - 1) < ans) {
                break;
            }
            for (ll i = j - 1; i >= 1; i--) {
                ans = max(ans, i * j - k * (a[i] | a[j]));
            }
        }
        cout<<ans<<endl;
    }
    return 0;
}
```