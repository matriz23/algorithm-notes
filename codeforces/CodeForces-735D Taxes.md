---
title: CodeForces-735D Taxes 
date: 2022-08-05 15:59:00
updated:
tags: [计算机, 算法, CodeForces, C++, 数论]
categories: CodeForces题解
cover: https://raw.githubusercontent.com/wsylab/wsyPicGo/main/img/photo-1510797246-b9c6ede0efa7.jpg
description: CodeForces-735D「Taxes」的思考与解答
---
### [CodeForces-735D Taxes](http://codeforces.com/problemset/problem/735/D)
### 题目大意
给定正整数 $n\geq 2$ , 把 $n$ 拆分成 $k$ 个 $\geq 2$ 的正整数 $n_1, n_2, ..., n_k$ , $k$ 任意, 使得 $n_i$ 不为自身的最大因子之和最小.
### Solution 1
Trick 题. 希望把 $n$ 拆分成尽可能少的素数之和. $n = 2$ 时, 无法拆分, 答案为 $1$ . $n\geq 4$ 且为偶数时, 根据哥德巴赫猜想 (已验证的部分) , 可以拆分成两个素数之和, 答案为 $2$ . 如果 $n$ 为奇数, 首先判断 $n$ 自身是否为素数, 如果是, 答案为 $1$ ;如果不是, 看 $n$ 能否拆成两个素数之和, 即判断 $n - 2$ 是否为素数, 如果是, 答案为 $2$ ; 如果不是, 则至少拆成三个素数, 拆成其它组合也不会更优, 答案为 $3$ .
代码如下:
```C++
#include <bits/stdc++.h>
using namespace std;
#define ll long long
const ll MOD = 1e9 + 7;

bool isPrime(int x) {
    for (int i = 2; i <= sqrt(x) + 1; i++) {
        if (x % i == 0) {
            return false;
        }
    }
    return true;
}

int main() {
    int n;
    cin>>n;
    if (n == 2) {
        cout<<1<<endl;
    }
    else if (n % 2 == 0) {
        cout<<2<<endl;
    }
    else {
        if (isPrime(n)) {
            cout<<1<<endl;
        }
        else if (isPrime(n - 2)) {
            cout<<2<<endl;
        }
        else {
            cout<<3<<endl;
        }
    }
    return 0;
}
```