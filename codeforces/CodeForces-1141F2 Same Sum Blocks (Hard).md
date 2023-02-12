---
title: CodeForces-1141F2 Same Sum Blocks (Hard) 
date: 2023-02-09 22:06:00
updated:
tags: [计算机, 算法, CodeForces, C++, 贪心]
categories: CodeForces题解
cover: 
description: CodeForces-1141F2「Same Sum Blocks (Hard)」的思考与解答
---
### [CodeForces-1141F2 Same Sum Blocks (Hard)](https://codeforces.com/problemset/problem/1141/F2)
### 题目大意
给定数组 $a_1, a_2,..., a_n$, 找出尽可能多的不相交子数组, 要求它们的和相同.
### Solution 1
因为 $n\leq 1500$ , 暴力计算所有子数组的和, 并用哈希表存储相同和的子数组的左右端点. 对于和相同的一系列子数组, 转化成为一个常见的问题: 选取最多的不相交区间. 这个问题可以用贪心解决.
代码如下:
```C++
#include "bits/stdc++.h"
using namespace std;
typedef pair<int, int> pii;
int main() {
    int n;
    cin >> n;
    int a[n + 1];
    for (int i = 1; i <= n; i++) {
        cin >> a[i];
    }
    map<int, vector<pii>> book;
    for (int i = 1; i <= n; i++) {
        int sum = 0;
        for (int j = i; j <= n; j++) {
            sum += a[j];
            book[sum].push_back(make_pair(i, j));
        }
    }
    int res = -1, ans = -1;
    for (auto& it : book) {
        sort(it.second.begin(), it.second.end(),
             [&](pii x, pii y) { return x.second < y.second; });
        int cnt = 0;
        int last_r = -1;
        for (auto [l, r] : it.second) {
            if (l > last_r) {
                cnt++;
                last_r = r;
            }
        }
        if (cnt > ans) {
            res = it.first;
            ans = cnt;
        }
    }
    cout << ans << endl;
    int last_r = -1;
    for (auto [l, r] : book[res]) {
        if (l > last_r) {
            cout << l << " " << r << endl;
            last_r = r;
        }
    }
    return 0;
}
```