---
title: CodeForces-788A Functions again 
date: 2022-07-14 11:39:00
updated:
tags: [计算机, 算法, CodeForces, C++, 动态规划]
categories: CodeForces题解
cover: https://raw.githubusercontent.com/wsylab/wsyPicGo/main/img/photo-1465146344425-f00d5f5c8f07
description: CodeForces-788A「Functions again」的思考与解答
---
### [CodeForces-788A Functions again](https://codeforces.com/problemset/problem/788/A)
### 题目大意
输入一个数组 $a_1, a_2,..., a_n$ , 定义一个函数, 函数如下:
$$ 
f[l,r]=\sum^{r-1}_{i=l}|a_i-a_{i+1}| × (-1)^{i-l}
$$ 
其中 $1\leq l,r\leq n$ . 求 $f$ 的最大值.

### Solution 1
注意到 $f$ 只和 $a$ 的差分有关, 我们求出差分数组 $d$ 之后, 根据 $l$ 的奇偶性分类讨论, 可以直接将 $d$ 的一部分变为相反数, 在新的 $d'$ 数组上计算最大子段和即可.

代码如下:
```C++
#include <bits/stdc++.h>
using namespace std;
const int MOD = 1e9 + 7;
int main() {
    int n;
    cin>>n;
    vector<long long> a(n + 1, 0);
    for (int i = 1; i <= n; i++) {
        cin>>a[i];
    }
    for (int i = 1; i < n; i++) {
        a[i] = abs(a[i] - a[i + 1]); // 转化为差分数组
    }
    // 计算奇数下标开头的函数值
    for (int i = 2; i <= n; i += 2) {
        a[i] = -1 * a[i]; // 此时偶数下标值反转
    }
    vector<long long> dp(n, 0); // dp[j] 以j为终点的最大子段和
    dp[1] = a[1];
    long long ans = a[1];
    for (int i = 2; i <= n - 1; i++) {
        dp[i] = max(dp[i - 1] + a[i], a[i]); // 经典最大子段和dp
        ans = max(ans, dp[i]); // ans其实需要和开头是奇数的比, 不过我们的处理过程已经保证了奇数开头的大于偶数开头的, 因此这里不会出错.
    }

    // 计算偶数下标开头的函数值
    for (int i = 1; i < n; i++) {
        a[i] = (-1) * a[i]; // 此时奇数下标值反转, 偶数之前反转了一次, 需要转回来
    }
    dp[1] = a[1];
    for (int i = 2; i < n; i++) {
        dp[i] = max(dp[i - 1] + a[i], a[i]);
        ans = max(ans, dp[i]);
    }
    cout<<ans<<endl;
    return 0;
}
```