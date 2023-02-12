---
title: CodeForces-1542B Plus and Multiply 
date: 2022-06-27 04:43:00
updated:
tags: [计算机, 算法, CodeForces, C++, 数学, 构造]
categories: CodeForces题解
cover: 
description: CodeForces-1542B「Plus and Multiply」的思考与解答
---
### [CodeForces-1542B Plus and Multiply](https://codeforces.com/problemset/problem/1542/B)

### 题目大意
给定正整数 $a, b$ , 考虑无限集 $S$ , $S$ 由如下规则构造:
- 1 $\in S$
- $\forall x\in S, x × a\in S, x + b\in S$
输入 $t$ ,代表有 $t$ 组测试数据, 每组数据输入正整数 $n, a, b$ , 判别 $n$ 是否在 $a, b$ 构造的 $S$ 中.
### Solution 1
对于 $x\in S$ , 可构造出 $x × a$ 和 $x + b$ , 满足 $x\equiv x × a\equiv x + b\ mod\ b$ , 由于初始元素为 $1$, 所以有 $n\equiv a^k\ mod\ b$ , 这是一个必要条件, 而当 $a^k\leq n$ 时, 可以构造出 $n$, 此时也充分. 本题在 $a = 1$ 时, 最好特判, 同时注意利用 $(n - 1)\equiv 0\ mod\ b$, 而不是 $n\equiv 1\ mod\ b$ 判断, 后者在 $b = 0$ 时会判断出错.
代码如下:
```C++
#include <bits/stdc++.h>
using namespace std;
int main() {
    int t;
    cin>>t;
    while (t--) {
        int n, a, b;
        cin>>n>>a>>b;
        bool flag = false;
        if (a == 1) {
            flag = ((n - 1) % b == 0);
        }
        else {
            long long pow = 1;
            while (pow <= n) {
                if (pow % b == n % b) {
                    flag = true;
                    break;
                }
                pow *= a;
            }
        }
        if (flag) {
            cout<<"Yes"<<endl;
        }
        else {
            cout<<"No"<<endl;
        }
    }
    return 0;
}
```