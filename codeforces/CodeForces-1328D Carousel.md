---
title: CodeForces-1328D Carousel 
date: 2022-07-29 11:49:00
updated:
tags: [计算机, 算法, CodeForces, C++, 构造, 贪心]
categories: CodeForces题解
cover: https://raw.githubusercontent.com/wsylab/wsyPicGo/main/img/photo-1561424412-6c2125ecb1cc
description: CodeForces-1328D「Carousel」的思考与解答
---
### [CodeForces-1328D Carousel](https://codeforces.com/problemset/problem/1328/D)
### 题目大意
一个环上有 $a_1, a_2, ..., a_n$ 这 $n$ 个数字, 要求对这些数字染色, 使得相邻的两个数字如果数字不同, 颜色也必须不同. 求满足一个条件最少染几种颜色, 并给出一个构造.
### Solution 1
首先要读清题意, 不要求同一种数字染同一种颜色, 只要求相邻的不同数字染不同颜色. 最简单的情况是只有一种数字, 染一种颜色就够了; 如果有两种及以上的数字, 需要考虑环的长度 $n$ 的奇偶性. 如果 $n$ 为偶数, 我们间隔染色 $ABABAB...$ 能使相邻的数字颜色不同, 必然满足条件; 如果 $n$ 是奇数, 从一点按 $ABABAB...$ 间隔染色, 最后染的一点会与这一点颜色相同, 只有这两点数字相同才合法. 为此, 搜索环上是否存在相邻的数字相同, 如果存在, 则用两种颜色交替染; 否则必须在最后添加一个新颜色, 共计使用 $3$ 种颜色.
代码如下:
```C++
#include "bits/stdc++.h"
using namespace std;
#define ll long long
typedef pair<ll, ll> pii;
int main() {
    int t;
    cin>>t;
    while (t--) {
        int n;
        cin>>n;
        vector<int> a(n, 0);
        set<int> book;
        for (int i = 0; i < n; i++) {
            cin>>a[i];
            book.insert(a[i]);
        }
        if (book.size() == 1) {
            cout<<1<<endl;
            for (int i = 0; i < n; i++) {
                cout<<1<<' ';
            }
            cout<<endl;
        }
        else {
            if (n % 2 == 0) {
                cout<<2<<endl;
                for (int i = 0; i < n; i += 2) {
                    cout<<1<<" "<<2<<" ";
                }
                cout<<endl;
            }
            else {
                bool flag = true;
                for (int i = 0; i < n; i++) {
                    if (a[i] == a[(i + 1) % n]) {
                        flag = false;
                        cout<<2<<endl;
                        int p = 1;
                        for (int j = 0; j < n; j++) {
                            cout<<p<<' ';
                            if (j != i) {
                                p = 3 - p;
                            }
                        }
                        cout<<endl;
                        break;
                    }
                }
                if (flag) {
                    cout<<3<<endl;
                    for (int i = 0; i < n - 1; i += 2) {
                        cout<<1<<" "<<2<<" ";
                    }
                    cout<<3<<endl;
                }

            }
        }
    }
    return 0;
}
```

### 如果同样的数字必须染同样的颜色?
我一开始就把题目理解成这样了. 在这种要求下, 需要以数字为顶点建图, 如果存在相邻关系就连一条边. 记这个图为 $G$ ,考虑 $G$ 的所有子图中最大的完全图 $K_m$ , 则把 $K_m$ 合法染色至少需要 $m$ 种颜色; 同时对于完全图上的每个顶点, 连接它的所有边可以拆分成一系列不超过 $K_m$ 的完全图, 可以用 $m$ 种颜色合法染色; 对于那些更边缘的点, 继续这样染色即可. 综上, $m$ 种颜色是最少的.