---
title: CodeForces-1043D Mysterious Crime 
date: 2022-08-03 15:19:00
updated:
tags: [计算机, 算法, CodeForces, C++]
categories: CodeForces题解
cover: https://raw.githubusercontent.com/wsylab/wsyPicGo/main/img/photo-1558056195-2e83b8e1338a
description: CodeForces-1043D「Mysterious Crime」的思考与解答
---
### [CodeForces-1043D Mysterious Crime](http://codeforces.com/problemset/problem/1043/D)
### 题目大意
给定 $m$ 个 $1, 2, ..., n$ 的排列, 求这些排列共同拥有的连续子数组的个数.
### Solution 1
用矩阵 $a[i][j], 0\leq i\leq m - 1, 0\leq j\leq n - 1$ 来记录这些排列. 共同的连续子数组也必须是 $a[0]$ 的连续子数组. 考虑一个包含了 $a[0][j]$ 的最长子数组是否包含 $a[0][j + 1]$ , 我们就需要知道在其它 $m - 1$ 个数组中, 值为 $a[0][j]$ 和 $a[0][j + 1]$ 的元素的下标 $pos_0$ 和 $pos_1$ , 如果 $pos_1 = pos_0 + 1$ 在这 $m - 1$ 个数组中都成立, 则我们的连续子数组可以继续延长. 因此, 在读取数组 $a$ 时, 用 $pos[i][a[i][j]] = j$ 来记录各个值得下标, 方便后续的判断. 我们从 $a[0][0]$ 遍历到 $a[0][n - 1]$ , 记录各个值所在的连续子数组最长的长度是多少. 对于一个长度为 $len$ 的连续子数组, 它一共拥有 $\frac{len\times (len + 1)}{2}$ 个子数组 (包括自身) , 我们累加就能得到答案.
代码如下:
```C++
#include <bits/stdc++.h>
using namespace std;
#define ll long long
int main() {
    int n, m;
    cin>>n>>m;
    vector<vector<int>> a(m, vector<int>(n, 0));
    vector<vector<int>> pos(m, vector<int>(n + 1, 0));
    for (int i = 0; i < m; i++) {
        for (int j = 0; j < n; j++) {
            scanf("%d", &a[i][j]);
            pos[i][a[i][j]] = j;
        }
    }
    ll cnt = 1;
    ll ans = 0;
    for (int j = 1; j < n; j++) {
        bool isValid = true;
        for (int i = 1; i < m && isValid; i++) {
            if (pos[i][a[0][j]] != pos[i][a[0][j - 1]] + 1) {
                isValid = false;
            }
        }
        if (isValid) {
            cnt++;
        }
        else {
            ans += cnt * (cnt + 1) / 2;
            cnt = 1;
        }
    }
    ans += cnt * (cnt + 1) / 2;
    cout<<ans<<endl;
    return 0;
}
```