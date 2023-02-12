---
title: CodeForces-235A LCM Challenge 
date: 2022-06-30 23:13:00
updated:
tags: [计算机, 算法, CodeForces, C++, 数论]
categories: CodeForces题解
cover: https://raw.githubusercontent.com/wsylab/wsyPicGo/main/img/photo-1496661274775-a86a124b9df3
description: CodeForces-235A「LCM Challenge」的思考与解答
---
### [CodeForces-235A LCM Challenge](https://codeforces.com/problemset/problem/235/A)
### 题目大意
输入 $n$ , 找到 $3$ 个不超过 $n$ 的正整数（可以相同），使得它们的 $LCM$ (最小公倍数)最大。输出最大的 $LCM$ .
### Solution 1
最小公倍数和最大公约数有如下关系: $LCM(n,m)=n∗m/GCD(n,m)$ , 我们需要寻找尽可能大的数, 同时它们的最大公约数尽可能小. 如果 $n$ 为奇数, $n, n - 1, n - 2$ 必然互质, 此时 $LCM$ 最大. 如果 $n$ 为偶数, 则 $n - 1$ 为奇数, 可以有 $n - 1, n - 2, n - 3$ . 有没有更大的呢? $n, n - 1, n - 2$ 不行( $n$ 与 $n - 2$ 都是偶数), 考虑 $n, n - 1, n - 3$, 我们计算 $n$ 与 $n - 3$ 是否互质, 若互质, 则取 $n, n - 1, n - 3$ , 否则取 $n - 1, n - 2, n - 3$ .
注意 $n = 1, 2$ 的情况需要单独处理.
代码如下:
```C++
#include <bits/stdc++.h>
using namespace std;
int main() {
    long long n;
    cin>>n;
    if (n <= 2) {
        cout<<n<<endl;
    }
    else {
        if (n % 2 == 1) {
            cout<<n * (n - 1) * (n - 2)<<endl;
        }
        else if (__gcd(n, (n - 3)) == 1) {
            cout<<n * (n - 1) * (n - 3)<<endl;
        }
        else {
            cout<<(n - 1)* (n - 2) * (n - 3)<<endl;
        }
    }
    return 0;
}
```