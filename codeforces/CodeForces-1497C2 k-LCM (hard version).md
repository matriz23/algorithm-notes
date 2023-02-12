---
title: CodeForces-1497C2 k-LCM (hard version) 
date: 2022-07-26 13:19:00
updated:
tags: [计算机, 算法, CodeForces, C++, 数论, 构造]
categories: CodeForces题解
cover: https://raw.githubusercontent.com/wsylab/wsyPicGo/main/img/photo-1528834342297-fdefb9a5a92b
description: CodeForces-1497C2「k-LCM (hard version)」的思考与解答
---
### [CodeForces-1497C2 k-LCM (hard version)](https://codeforces.com/problemset/problem/1497/C2)
### 题目大意
给定正整数 $n, k$ , $3\leq k\leq n$ , 构造数组 $a_1, a_2, ..., a_k$ , 使得 $\sum_{1\leq i\leq k}a_i = n$ 且 $lcm(\{a_i|1\leq i\leq n\}) \leq \lfloor \frac{n}{2}\rfloor$ .
### Solution 1
因为 $lcm(1, a) = a$ , 所以先输出 $k - 3$ 个 $1$ , 再考虑把 $m = n - (k - 3)$ 拆分成 $3$ 个数. 
- 若 $m = 2t + 1$ , 则拆分为 $1, t, t$ ; 
- 如果 $m = 4t + 2$ , 则拆分为 $2, 2t, 2t$ ; 
- 如果 $m = 4t + 4$ , 则拆分为 $t + 1, t + 1, 2t + 2$ .  
  
代码如下:
```C++
#include "bits/stdc++.h"
using namespace std;
#define ll long long
int main() {
    int t;
    cin>>t;
    while (t--) {
        int n, k;
        cin>>n>>k;
        for (int i = 1; i <= k - 3; i++) {
            cout<<1<<" ";
        }
        n = n - (k - 3);
        if (n % 2 == 1) {
            cout<<1<<" "<<(n - 1) / 2<<" "<<(n - 1) / 2<<endl;
        }
        else if (n % 4 == 2) {
            cout<<2<<" "<<(n - 2) / 2<<" "<<(n - 2) / 2<<endl;
        }
        else {
            cout<<(n - 4) / 4 + 1<<" "<<(n - 4) / 4 + 1<<" "<<(n - 4) / 2 + 2<<endl;
        }
    }
    return 0;
}
```