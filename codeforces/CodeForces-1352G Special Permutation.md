---
title: CodeForces-1352G Special Permutation 
date: 2022-06-27 23:42:00
updated:
tags: [计算机, 算法, CodeForces, C++, 构造]
categories: CodeForces题解
cover: https://raw.githubusercontent.com/wsylab/wsyPicGo/main/img/photo-1524687501140-49edc1efd2d6
description: CodeForces-1352G「Special Permutation」的思考与解答
---
### [CodeForces-1352G Special Permutation](https://codeforces.com/problemset/problem/1352/G)
### 题目大意
对于$1, 2, ..., n$ 的排列 $p$ , 如果 $\forall 1\leq i\leq n -1, 2\leq \vert p_i - p_{i + 1}\vert \leq 4$ , 则称其为特殊排列. 输入 $n$ , 如果存在特殊排列, 返回任意一个排列, 否则返回 $-1$ . 

### Solution 1
首先验证 $n = 1, 2, 3$ 时不存在特殊排列, $n = 4$ 时有特殊排列 $3, 1, 4, 2$. 对于 $n > 4$ , 我们把新增的奇数放到左边, 新增的偶数放到右边即可. 
代码如下:
```C++
#include <bits/stdc++.h>
using namespace std;
int main() {
    int t;
    cin>>t;
    while (t--) {
        int n;
        cin>>n;
        if (n < 4) {
            cout<<-1<<endl;
        }
        else {
            for (int i = (n - 1) / 2 * 2 + 1; i >= 1; i -= 2) {
                cout<<i<<" ";
            }
            cout<<4<<" "<<2<<" ";
            for (int i = 6; i <= n / 2 * 2; i += 2) {
                cout<<i<<" ";
            }
            cout<<endl;
        }
    }
    return 0;
}
```