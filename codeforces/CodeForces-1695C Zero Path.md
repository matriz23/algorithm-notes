---
title: CodeForces-1695C Zero Path 
date: 2023-02-10 20:33:00
updated:
tags: [计算机, 算法, CodeForces, C++, 动态规划]
categories: CodeForces题解
cover: 
description: CodeForces-1695C「Zero Path」的思考与解答
---
### [CodeForces-1695C Zero Path](https://codeforces.com/problemset/problem/1695/C)
### 题目大意
给定一个 $n\times m$ 的矩阵, 矩阵元素由 $1$ 和 $-1$ 构成. 判断是否存在从 $(0, 0)$ 到 $(n - 1, m - 1)$ 的路径满足路径和为 $0$ .
### Solution 1
从 $(0, 0)$ 到 $(n - 1, m - 1)$ 的路径长为 $n + m - 1$ , 只有当 $n + m - 1$ 为偶数时, 路径和才有可能为 $0$ . 
把路径中的某两步交换一下, 会使得路径长度 $+2$ , 不变或 $-2$ . 求出从 $(0, 0)$ 到 $(n - 1, m - 1)$ 的最大路径和与最小路径和, 类似介值定理, 此时存在路径和为 $0$ 的路径当且仅当最大路径和 $\geq 0$ 且最小路径和 $\leq 0$ .
代码如下:
```C++
#include <bits/stdc++.h>
using namespace std;

int main() {
    int t;
    cin >> t;
    while (t--) {
        int n, m;
        cin >> n >> m;
        int a[n][m];
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < m; j++) {
                cin >> a[i][j];
            }
        }
        bool ans = false;
        if ((n + m) & 1) {
            int max_dp[n][m], min_dp[n][m];
            max_dp[0][0] = min_dp[0][0] = a[0][0];
            for (int i = 1; i < n; i++) {
                max_dp[i][0] = min_dp[i][0] = max_dp[i - 1][0] + a[i][0];
            }
            for (int j = 1; j < m; j++) {
                max_dp[0][j] = min_dp[0][j] = max_dp[0][j - 1] + a[0][j];
            }
            for (int i = 1; i < n; i++) {
                for (int j = 1; j < m; j++) {
                    max_dp[i][j] =
                        max(max_dp[i - 1][j], max_dp[i][j - 1]) + a[i][j];
                    min_dp[i][j] =
                        min(min_dp[i - 1][j], min_dp[i][j - 1]) + a[i][j];
                }
            }
            ans = max_dp[n - 1][m - 1] >= 0 && min_dp[n - 1][m - 1] <= 0;
        }
        printf("%s\n", ans ? "YES" : "NO");
    }
    return 0;
}
```