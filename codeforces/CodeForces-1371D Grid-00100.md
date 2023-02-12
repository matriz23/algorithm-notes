---
title: CodeForces-1371D Grid-00100 
date: 2022-07-21 11:09:00
updated:
tags: [计算机, 算法, CodeForces, C++, 构造, 贪心]
categories: CodeForces题解
cover: https://raw.githubusercontent.com/wsylab/wsyPicGo/main/img/photo-1606606767399-01e271823a2e
description: CodeForces-1371D「Grid-00100」的思考与解答
---
### [CodeForces-1371D Grid-00100](https://codeforces.com/problemset/problem/1371/D)
### 题目大意
记 $R_i$ 为矩阵行和, $C_j$ 为矩阵列和. 给定正整数 $n, k, k\leq n^2$ , 构造一个形状为 $n \times n$ 且有 $k$ 个元素是 $1$ 的 $01$ 矩阵 $A$ , 使得 $f(A) = (\underset{1\leq i\leq n}{max}\ R_i - \underset{1\leq i\leq n}{min}\ R_i)^2 + (\underset{1\leq j\leq n}{max}\ C_j - \underset{1\leq j\leq n}{min}\ C_j)^2$ 最小.
### Solution 1
目标: 尽可能减小行和列的差异. 如果 $n = k$ , 容易想到按对角线排布 $1$ , 这样 $f(A) = 0$ .如果 $n = 2k$ 呢? 我们在对角线元素下方再分别布置 $1$ 同样能使 $f(A) = 0$ (这其实是模 $n$ 意义下的另一条对角线, 一共有 $n$ 条) . 
沿这些对角线放置 $1$ , 若 $n = mk, 0\leq m\leq n$ , $f(A) = 0$ , 达到最小值. 当$n = mk + d, 0\leq m\leq n - 1, 1\leq d\leq n - 1$ 时, 行列最大值都是 $m + 1$ , 最小值都是 $m$ , 这已经是最平均(最优)的分配了, 此时 $f(A) = 2$ .
代码如下:
```C++
#include <bits/stdc++.h>
using namespace std;
void print_grim(vector<vector<int>> a) {
    int n = a.size();
    int row_min = INT_MAX, row_max = 0, col_min = INT_MAX, col_max = 0;
    for (int i = 0; i < n; i++) {
        int row_sum = 0;
        for (int j = 0; j < n; j++) {
            row_sum += a[i][j];
        }
        row_min = min(row_min, row_sum);
        row_max = max(row_max, row_sum);
    }
    for (int j = 0; j < n; j++) {
        int col_sum = 0;
        for (int i = 0; i < n; i++) {
            col_sum += a[i][j];
        }
        col_min = min(col_min, col_sum);
        col_max = max(col_max, col_sum);
    }
    cout<<(row_max - row_min) * (row_max - row_min) + (col_max - col_min) * (col_max - col_min)<<endl;
    for (int i = 0; i < n; i++) {
        for (int j = 0; j < n; j++) {
            cout<<a[i][j];
        }
        cout<<endl;
    }
}

int main() {
    int t;
    cin>>t;
    while (t--) {
        int n, k;
        cin>>n>>k;
        vector<vector<int>> a(n, vector<int>(n, 0));
        for (int i = 0; i < n && k > 0; i++) {
            for (int j = 0; j < n && k > 0; j++) {
                a[j][(j + i) % n] = 1;
                k--;
            }
        }
        print_grim(a);
    }
    return 0;
}
```