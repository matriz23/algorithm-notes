---
title: CodeForces-1583C Omkar and Baseball 
date: 2022-07-13 21:30:00
updated:
tags: [计算机, 算法, CodeForces, C++, 构造, 错排]
categories: CodeForces题解
cover: https://raw.githubusercontent.com/wsylab/wsyPicGo/main/img/photo-1578432014316-48b448d79d57
description: CodeForces-1583C「Omkar and Baseball」的思考与解答
---
### [CodeForces-1583C Omkar and Baseball](https://codeforces.com/problemset/problem/1372/C)
### 题目大意
给你一个 $1, 2,..., n$ 的排列, 每次操作可以选择一段区间进行排列, 要求排列后不能有元素留在自己的原位上, 求把排列变为升序的最少操作次数.

### Solution 1
首先考虑最简单的情况, 即原排列就是升序的, 此时不需要操作. 对于其它情况, 我们先分析一次操作能带来什么. 很明显, 一段区间如果存在一个元素在自己应在的位置上, 那么这次操作将会破坏这一点. 因此, 我们尽量考虑对全部错排的区间进行操作, 一次操作就可以让这个区间有序. 对于原排列, 如果错位的数字刚好构成一段连续的区间, 那么我们可以用 $1$ 次操作解决. 对于其它情况, 很明显至少要 $2$ 次操作. 事实上, $2$ 次操作总是足够的. 我们第一次选定所有元素进行操作, 让其完全构成一个错排(有序元素置换, 错位元素仍能错排成一个新的错排, 限于篇幅不作严格证明), 第 $2$ 次操作再归位即可.
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
        int cnt = 0;
        vector<int> a(n + 1, 0);
        vector<int> b;
        for (int i = 1; i <= n; i++) {
            cin>>a[i];
            if (a[i] == i) {
                b.push_back(i);
            }
            
        }
        if (b.size() == n) {
            cout<<0<<endl;
        }
        else {
            int cnt = 0;
            int p = 1;
            while (a[p] == p) {
                p++;
            }
            while (a[p] != p && p <= n) {
                cnt++;
                p++;
            }
            if (cnt == n - b.size()) {
                cout<<1<<endl;
            }
            else {
                cout<<2<<endl;
            }  
        }
    }
    return 0;
}
```

### 心灵对换机
看到这道题, 我很快想到， Matrix67在其著作《浴缸里的惊叹》中提到了一个《Futurama》中一个很精彩的数学问题: 心灵对换机, 这篇文章最早收录在他的博客中, 链接在此: [Futurama S06E10中的数学问题](http://www.matrix67.com/blog/archives/3570). 这是一个看上去复杂得多的问题, 但同样拥有惊人的结论与精巧的解法.
#### 问题描述
有一台"心灵对换机"可以交换两个人的心灵, 但不同重复作用于同一对人(说成身体更准确一点)上. 现在给出 $n$ 个心灵错乱的人和他们之前所有的心灵交换记录, 为了让所有人的心灵重新归位, 需要引进多少个人, 才能使得合法的方案存在呢?
#### 解决方案
这是一个构造性的方案.
引进 $2$ 个人 $x, y$ , 同时考虑一个完全乱序的环, 不妨设为 $2, 3,..., k, 1$ . 按照如下策略交换:
$$
2\  3\  4\  5\  6\  …\  k\   1\  x\  y\\
x\  3\  4\  5\  6\  …  k\   1\  2\  y\\
x\  y\  4\  5\  6\  …  k\   1\  2\  3\\
x\  y\  3\  5\  6\  …  k\   1\  2\  4\\
x\  y\  3\  4\  6\  …  k\   1\  2\  5\\
x\  y\  3\  4\  5\  …  k\   1\  2\  6\\
…\ …\ …\\
x\  y\  3\  4\  5\  … k-1\  1\  2\  k\\
x\  y\  3\  4\  5\  … k-1\  k\  2\  1\\
x\  2\  3\  4\  5\  … k-1\  k\  y\  1\\
1\  2\  3\  4\  5\  … k-1\  k\  y\  x\\
$$
最后有需要再对换 $x, y$ 即可.