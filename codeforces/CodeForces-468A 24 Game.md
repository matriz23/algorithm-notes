---
title: CodeForces-468A 24 Game 
date: 2022-07-14 13:44:00
updated:
tags: [计算机, 算法, CodeForces, C++, 构造]
categories: CodeForces题解
cover: https://raw.githubusercontent.com/wsylab/wsyPicGo/main/img/photo-1541278107931-e006523892df
description: CodeForces-468A「24 Game」的思考与解答
---
### [CodeForces-468A 24 Game](https://codeforces.com/problemset/problem/468/A)
### 题目大意
有一个长度为 $n$ 的序列 $1, 2, ..., n$ , 每次可以选择两个数 $a, b$ 删掉, 再从 $a + b$ , $a - b$, $a × b$ 中选择一个加入序列. 经过 $n - 1$ 次操作后, 序列中只会剩下一个数, 如果这个数可能为 $24$ , 输出 $YES$ 并给出构造方案, 否则输出 $NO$ .
### Solution 1
由于任何数 $×1$ 都为其本身, 而 $1$ 可以由相邻的数相减得到, 我们可以轻易地用 $n = k$ 的操作方案构造出 $k + 2$ 的操作方案. 因此我们检验比较小的 $n$ 即可. 容易发现 $n = 1, 2, 3$ 时不存在合法的构造方案, 而 $n = 4, 5$ 时存在, 故 $\forall n \geq 4$ , 合法的构造方案都是存在的.
代码如下:
```C++
#include <bits/stdc++.h>
using namespace std;
int main() {
    int n;
    cin>>n;
    if (n < 4) {
        cout<<"NO"<<endl;
    }
    else if (n % 2 == 0) {
        cout<<"YES"<<endl;
        for (int i = 5; i <= n; i += 2) {
            cout<<i + 1<<" - "<<i<<" = "<<1<<endl;
            cout<<1<<" * "<<1<<" = "<<1<<endl;
        }
        cout<<1<<" * "<<2<<" = "<<2<<endl;
        cout<<2<<" * "<<3<<" = "<<6<<endl;
        cout<<6<<" * "<<4<<" = "<<24<<endl;
    }
    else if (n % 2 == 1) {
        cout<<"YES"<<endl;
        for (int i = 6; i <= n; i += 2) {
            cout<<i + 1<<" - "<<i<<" = "<<1<<endl;
            cout<<1<<" * "<<1<<" = "<<1<<endl;
        }
        cout<<4<<" * "<<5<<" = "<<20<<endl;
        cout<<3<<" - "<<1<<" = "<<2<<endl;
        cout<<2<<" + "<<2<<" = "<<4<<endl;
        cout<<4<<" + "<<20<<" = "<<24<<endl;
    }
    return 0;
}
```