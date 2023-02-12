---
title: CodeForces-1359C Mixing Water 
date: 2022-07-26 12:10:00
updated:
tags: [计算机, 算法, CodeForces, C++, 思维]
categories: CodeForces题解
cover: https://raw.githubusercontent.com/wsylab/wsyPicGo/main/img/photo-1564890369478-c89ca6d9cde9
description: CodeForces-1359C「Mixing Water」的思考与解答
---
### [CodeForces-1359C Mixing Water](https://codeforces.com/problemset/problem/1359/C)
### 题目大意
你有无限多杯热水和冷水, 热水温度为 $h$ , 冷水温度为 $c$ , 同时有一个容量无限大的空桶. 你的目标是让桶内的水温度到达 $t$ , $t$ 满足 $c\leq t\leq h$ . 为此, 你可以按下列方式向桶中倒水:
- 第 $1$ 次, 倒入一杯热水;
- 第 $2$ 次, 倒入一杯冷水;
- 第 $3$ 次, 倒入一杯热水;
- 第 $4$ 次, 倒入一杯冷水;
- $\dots$

至少倒一杯水, 求使得桶内温度尽可能接近 $t$ , 最少需要倒几次水.

### Solution 1
根据题意, 必须倒 $m$ 杯热水, $m$ 或 $m - 1$ 杯冷水.
如果 $h = t$ , 倒一杯热水刚好满足要求;
如果 $h + c \geq 2\times t$ , 则倒了 $2 × m$ 杯水后, 再倒热水会让温度升高, 进一步偏离目标 $t$ , 因此倒 $2$ 杯水达到最优;
如果 $h + c < 2 × t$ , 假设已经倒了 $2 \times m$ 杯水, 显然下一杯水会让温度升高, 升高的幅度从随 $m$ 增大而减小. 我们可以二分搜索一个最优的 $m$ , 也可以从单调性入手, 整数 $m$ 必定在浮点数解的两侧. 比较 $\lfloor \frac{t - h}{h + c - 2\times t}  \rfloor$ 与 $\lfloor \frac{t - h}{h + c - 2\times t}  \rfloor + 1$ 时的温度即可. 
代码如下:
```C++
#include "bits/stdc++.h"
using namespace std;
#define ll long long
int main() {
    int T;
    cin>>T;
    while (T--) {
        int h, c, t;
        cin>>h>>c>>t;
        h = h - t;
        c = c - t;
        if (h == 0) {
            cout<<1<<endl;
        }
        else if (h + c >= 0) {
            cout<<2<<endl;
        }
        else {
            int m = floor(-1.0 * h / (h + c));
            if (((m + 1) * (h + c) - c) * (2 * m + 3) + ((m + 2) * (h + c) - c) * (2 * m + 1) > 0) {
                cout<<2 * m + 3<<endl;
            }
            else {
                cout<<2 * m + 1<<endl;
            }
        }
    }
    return 0;
}
```
