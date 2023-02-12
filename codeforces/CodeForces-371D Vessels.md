---
title: CodeForces-371D Vessels 
date: 2022-09-12 13:56:00
updated:
tags: [计算机, 算法, CodeForces, C++, 并查集]
categories: CodeForces题解
cover: https://raw.githubusercontent.com/wsylab/wsyPicGo/main/img/photo-1506899686410-4670690fccef
description: CodeForces-371D「Vessels」的思考与解答
---
### [CodeForces-371D Vessels](https://codeforces.com/problemset/problem/371/D)
### 题目大意
有 $n$ 个从上到下摆放的容器, 每个容器的容量为 $a_i$ . 一个容器中溢出的水会流到下一个容器中, 最后一个容器的水则会流到地上. 有 $m$ 次倒水和查询的操作, 输出结果.
### Solution 1
我们需要快速地找到每个容器后面一个可行的容器是什么, 注意到连续的盛满水的容器可以看作一组, 我们可以使用并查集来记录. 记 $pa[i] + 1$ 为 $i$ 之后第一个非空的容器. 每次加水, 如果 $i$ 满了, 就将 $i - 1$ 的父节点设为 $i$ 的父节点( $i$ 上面的容器一定会流到 $i - 1$ , $i - 1$ 也会先流到 $i$), 同时继续考虑 $i = pa[i] + 1$ . 这种操作下满了的容器不会再被考虑到, 总体复杂度为 $O(m + n)$ .
代码如下:
```C++
#include <bits/stdc++.h>
using namespace std;
#define ll long long
const ll MOD = 1e9 + 7;
int pa[200010];

void init(int n){
    for(int i = 1; i <= n + 1; i++){
        pa[i] = i;        //存放每个结点的结点（或双亲结点）
    }
}

int find(int i) {
    if (pa[i] == i) {
        return i;
    }
    else {
        pa[i] = find(pa[i]);
        return pa[i];
    }
}

void merge(int i, int j) {
    pa[find(j)] = find(i);
}

int main() {
    int n;
    cin>>n;
    vector<ll> a(n + 2, 0), b(n + 2, 0);
    for (int i = 1; i <= n; i++) {
        cin>>a[i];
    }
    int m;
    cin>>m;
    init(n);
    while (m--) {
        int type;
        cin>>type;
        if (type == 1) {
            int p, x;
            cin>>p>>x;
            b[p] += x;
            while (p <= n && b[p] > a[p]) {
                merge(p, p - 1);
                int np = find(p) + 1;
                b[np] += b[p] - a[p];
                b[p] = a[p];
                p = np;
            }
        }
        else {
            int k;
            cin>>k;
            cout<<b[k]<<endl;
        }
    }
    return 0;
}
```