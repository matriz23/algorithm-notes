---
title: CodeForces-1096D Easy Problem 
date: 2022-07-18 14:25:00
updated:
tags: [计算机, 算法, CodeForces, C++]
categories: CodeForces题解
cover: https://raw.githubusercontent.com/wsylab/wsyPicGo/main/img/photo-1544672531-10bf64a0da46
description: CodeForces-1096D「Easy Problem」的思考与解答
---
### [CodeForces-1096D Easy Problem](https://codeforces.com/problemset/problem/1096/D)
### 题目大意
给定一个字符串 $s$ 和删除 $s[i]$ 的代价 $a[i]$ , 求使得 $s$ 不含有子序列(不要求连续) $hard$ 的最小代价.

### Solution 1
对于字符串子序列的问题, 我们考虑利用动态规划进行求解. 对于字符 $s[i]$ , 它对于我们字符串匹配的影响如下:
- 如果 $s[i]\in hard$ , 则匹配数可能增加
- 如果 $s[i]\not \in hard$ , 则匹配数不变

因此我们计算基于匹配数的最小代价. 记 $dp[i][j]$ 为 $s$ 的前 $i + 1$ 个字符中, 至多匹配 $hard$ 的前 $j$ 个字符的代价. 如果 $s[i] = hard[j]$ , 如果我们付出 $a[i]$ 的代价删除它, 则 $dp[i][j]$ 由 $dp[i - 1][j]$ 转移得到; 否则要满足不构成字符匹配条件, 只能由 $dp[i - 1][j - 1]$ 转移得到. 有如下状态转移方程:
$$
dp[i][j] = max(dp[i - 1][j] + a[i], dp[i - 1][j - 1])
$$

进一步优化可以使用滚动数组.
代码如下:
```C++
#include <bits/stdc++.h>
using namespace std;
const string HARD = "hard";
int main() {
    int n;
    string s;
    cin>>n>>s;
    vector<long long> a(n, 0);
    for (int i = 0; i < n; i++) {
        cin>>a[i];
    }
    vector<vector<long long>> dp(n, vector<long long>(4, 0));
    dp[0][0] = (s[0] == HARD[0])? a[0]: 0;;
    dp[0][1] = 0;
    dp[0][2] = 0;
    dp[0][3] = 0;
    for (int i = 1; i < n; i++) {
        for (int j = 0; j < 4; j++) {
            if (s[i] == HARD[j]) {
                dp[i][j] = (j == 0)? dp[i - 1][j] + a[i]: min(dp[i - 1][j] + a[i], dp[i - 1][j - 1]);
            }
            else {
                dp[i][j] = dp[i - 1][j];
            }
        }
    }
    cout<<dp[n - 1][3]<<endl;
    return 0;
}
```