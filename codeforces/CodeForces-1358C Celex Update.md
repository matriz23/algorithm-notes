---
title: CodeForces-1358C Celex Update 
date: 2022-07-25 14:08:00
updated:
tags: [计算机, 算法, CodeForces, C++, 数学]
categories: CodeForces题解
cover: https://raw.githubusercontent.com/wsylab/wsyPicGo/main/img/photo-1621619856624-42fd193a0661
description: CodeForces-1358C「Celex Update」的思考与解答
---
### [CodeForces-1358C Celex Update](https://codeforces.com/problemset/problem/1358/C)
### 题目大意
有一个如下图所示的矩阵.
![](https://raw.githubusercontent.com/wsylab/wsyPicGo/main/img/ab3c49666e913d52a14ebf7f09d741f3f712bacb.png)
给定 $x_1,y_1, x_2,y_2$ , 求 $(x_1, y_1)$ 到 $(x_2, y_2)$ 的路径中, 不同路径和的个数.

### Solution 1
这道题是结论题. 对于一条向右再向下的路径, 我们可以把把右上角的拐角翻折(对于初始的情况来说, 就是 $(x_2, y_1)\rightarrow (x_2 - 1, y_1 + 1)$) , 这会使得路径和 $+1$ . 通过这种操作我们可以获得最小路径和和最大路径和之间的每个值, 共有 $(x_2 - x_1)\times (y_2 - y_1) + 1$ 个值.
代码如下:
```C++
#include "bits/stdc++.h"
using namespace std;
#define ll long long
int main() {
    int t;
    cin>>t;
    while (t--) {
        ll x1, y1, x2, y2;
        cin>>x1>>y1>>x2>>y2;
        cout<<(x2 - x1) * (y2 - y1) + 1<<endl;
    }
    return 0;
}
```