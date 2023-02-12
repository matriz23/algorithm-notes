---
title: CodeForces-1389B Array Walk 
date: 2022-07-02 14:05:00
updated:
tags: [计算机, 算法, CodeForces, C++, 动态规划]
categories: CodeForces题解
cover: https://raw.githubusercontent.com/wsylab/wsyPicGo/main/img/photo-1507327160427-b29d8a92f971
description: CodeForces-1389B「Array Walk」的思考与解答
---
### [CodeForces-1389B Array Walk](https://codeforces.com/problemset/problem/1389/B)
### 题目大意
## 题面翻译
一共有 $n$ 个格子, 每个格子都有它的分值 $a_x$. 当你到达第 $x$ 个格子就能获得 $a_x$ 得分. 初始时你站在第 $1$ 个格子, 每一次移动你可以选择向左或向右, 特别地, 向左移动的次数不能超过 $z$. 求走 $k$ 步所能得到的最大得分.
### Solution 1
每一次移动选择向左或向右, 这提示我们或许可以采用动态规划的解法. 由于可以向左走的特点, 可以选择当前位置再分析状态, 也可以选择直接以向左走的步数、向右走的步数作为状态. 这里我们选择后一种. 记 $dp[i][j]$ 为向右走了 $i$ 步, 向左走了 $j$ 步能得到的最大得分, 我们最终要求的就是 
$$
\underset{i + j = k}{max}\ dp[i][j]
$$
在 $dp[i][j]$ 的前一步, 我们可以向右走, 也可以向左走, 都会走到 $a[i - j]$ 上. 有如下状态转移方程:
$$
dp[i][j] = max(dp[i - 1][j], dp[i][j - 1]) + a[i - j]
$$
这里的递推是不是单纯按行/列进行, 而是沿对角线进行的, 在写循环部分代码时需要注意.
代码如下:
```C++
#include <bits/stdc++.h>
using namespace std;
int main() {
    int t;
    scanf("%d", &t);
    while (t--) {
        // cout<<"dot 0"<<endl;
        int n, k, z;
        scanf("%d%d%d", &n, &k, &z);
        vector<int> a(n, 0);
        for (int i = 0; i < n; i++) {
            scanf("%d", &a[i]);
        }
        int ans = 0;
        vector<vector<int>> dp(n + 1, vector<int>(6, 0));
        dp[0][0] = a[0];
        
        for (int l = 1; l <= k; l++) {
            for (int i = l; i >= l - z; i--) {
                int j = l - i;
                if (j > i || j > z) {
                    continue;
                }
                if (i != 0 && j != 0) {
                    dp[i][j] = max(dp[i - 1][j] + a[i - j], dp[i][j - 1] + a[i - j]);
                }
                else if (i == 0 && j != 0) {
                    dp[i][j] = dp[i][j - 1] + a[i - j];
                }
                else if (i != 0 && j == 0) {
                    dp[i][j] = dp[i - 1][j] + a[i - j];
                }
                if (i + j == k) {
                    ans = max(ans, dp[i][j]);
                }
            }
        }
        cout<<ans<<endl;
    }
    return 0;
}
```
