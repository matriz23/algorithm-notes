---
title: CodeForces-1336A Linova and Kingdom 
date: 2022-07-22 12:01:00
updated:
tags: [计算机, 算法, CodeForces, C++]
categories: CodeForces题解
cover: https://raw.githubusercontent.com/wsylab/wsyPicGo/main/img/photo-1563018259-f7bf99f6b1b0
description: CodeForces-1336A「Linova and Kingdom」的思考与解答
---
### [CodeForces-1336A Linova and Kingdom](https://codeforces.com/problemset/problem/1336/A)
### 题目大意
给定 $n$ 个节点的有根树, 根是 $1$ 号节点. 你可以选择 $k$ 个节点将其设置为工业城市, 其余设置为旅游城市. 对于一个工业城市, 定义它的幸福值为工业城市到根的路径经过的旅游城市的数量. 求所有工业城市的幸福值之和的最大可能值.
### Solution 1
看到题目和样例, 第一反应当然是贪心, 优先选择深度最大的节点. 但按这种策略操作时, 如果选择的新工业城市其子节点已经是工业城市, 这个操作会影响到所有的子节点. 针对这一点进行修正, 选择 $i$ 节点变为工业城市的价值变化量为该节点提供的幸福值与子树中除自身外所有节点幸福值的减少量之差, 即 $val[i] = (d[i] - 1) - (cs[i] - 1) = d[i] - cs[i]$ , 其中 $d[i]$ 是深度, $cs[i]$ 为子树(包括自己)节点数量. 依据 $val$ 大小选择同时也能保证在选择一个节点时, 其子节点已经被选过了, 定义是满足的.
根据边建图时, 我们用 $g[i]$ 存储 $i$ 节点相连的边. 遍历子节点时, 为了区分 $i$ 的子节点与父节点, $dfs()$ 函数被设计成传入父节点参数与子节点参数. 初始使用 $dfs(0, 1)$ , 规定 $0$ 是 $1$ 的父节点.
代码如下:
```C++
#include <bits/stdc++.h>
using namespace std;
vector<int> g[200010];
long long d[200010], cs[200010], val[200010];
void dfs(int p, int c) {
    d[c] = d[p] + 1;
    cs[c] = 1;
    for (int i = 0; i < g[c].size(); i++) {
        if (g[c][i] != p) {
            dfs(c, g[c][i]);
            cs[c] += cs[g[c][i]];
        }
    }
}

int main() {
    int n, k;
    cin>>n>>k;
    for (int i = 1; i < n; i++) {
        int u, v;
        cin>>u>>v;
        g[u].push_back(v);
        g[v].push_back(u);
    }
    dfs(0, 1);
    for (int i = 1; i <= n; i++) {
        val[i] = d[i] - cs[i];
    }
    sort(val + 1, val + n + 1, [&](int a, int b) {
        return a > b;
    });
    long long ans = 0;
    for (int i = 1; i <= k; i++) {
        ans += val[i];
    }
    cout<<ans<<endl;
    return 0;
}
```