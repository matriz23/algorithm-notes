---
title: CodeForces-1469C Building a Fence 
date: 2022-08-24 09:48:00
updated:
tags: [计算机, 算法, CodeForces, C++, 动态规划]
categories: CodeForces题解
cover: https://raw.githubusercontent.com/wsylab/wsyPicGo/main/img/photo-1583805978118-ba9a81ac1399.avif
description: CodeForces-1469C「Building a Fence」的思考与解答
---
### [CodeForces-1469C Building a Fence](https://codeforces.com/problemset/problem/1469/C)
### 题目大意
给定有 $n$ 个长为 $1$ , 高度为 $k$ 的篱笆, 你需要给一块长为 $n$ 的土地布设篱笆. 这块土地是凹凸不平的, 第 $i$ 个部分的土地的高度为 $h_i$ .
篱笆的分布需要遵守以下要求:
- 第一块和最后一块篱笆需要正好放在地面上;
- 对于两块相邻的篱笆, 接触面积的长度至少为 $1$ ;
- 中间的篱笆可以放在地面上, 也可以悬空, 但至多高出地面 $k - 1$ 的高度. 
判断能否按要求铺设篱笆.
### Solution 1
根据规则, 如果从前向后铺设篱笆, 每一块篱笆的可行范围都是连续的. 分别用 $ub[i], lb[i]$ 记录篱笆顶点的最高范围和最低范围, 则有如下状态转移方程:
$$
\begin{cases}
ub[i] = min(ub[i - 1] - 1 + k, h[i] + k - 1 + k)\\
lb[i] = max(h[i] + k, lb[i - 1] - k + 1)
\end{cases}
$$
如果某一处 $ub < lb$ , 那么就不存在合法的铺设方式; 对于最后一块地, 需要满足 $lb[n - 1] = h[n - 1] + k$ .
代码如下:
```C++
#include <bits/stdc++.h>
using namespace std;
#define ll long long
const ll MOD = 1e9 + 7;
int main() {
    int t;
    cin>>t;
    while (t--) {
        int n, k;
        cin>>n>>k;
        vector<int> h(n, 0);
        for (int i = 0; i < n; i++) {
            cin>>h[i];
        }
        vector<int> ub(n, 0), lb(n, 0);
        ub[0] = h[0] + k;
        lb[0] = h[0] + k;
        bool isValid = true;
        for (int i = 1; i < n && isValid; i++) {
            ub[i] = min(ub[i - 1] - 1 + k, h[i] + k - 1 + k);
            lb[i] = max(h[i] + k, lb[i - 1] - k + 1);
            if (ub[i] < lb[i]) {
                isValid = false;
            }
        }
        if (lb[n - 1] == h[n - 1] + k && isValid) {
            cout<<"YES"<<endl;
        }
        else {
            cout<<"NO"<<endl;
        }
    }
    return 0;
}
```
