---
title: CodeForces-431C k-Tree 
date: 2022-07-21 17:23:00
updated:
tags: [计算机, 算法, CodeForces, C++, 动态规划]
categories: CodeForces题解
cover: https://raw.githubusercontent.com/wsylab/wsyPicGo/main/img/photo-1559133171-df1ff588e59f
description: CodeForces-431C「k-Tree」的思考与解答
---
### [CodeForces-431C k-Tree]()
### 题目大意
有一颗无限大的 $k$ 叉树, 每个节点都有 $k$ 个子节点, 边权分别是 $1, 2, ..., k$ . 规定路径价值为经过的边的边权之和. 求从上至下、价值为 $n$ 且至少经过一条边权至少为 $d$ 的路径数量.
### Solution 1
这道题其实和树没什么关系. 我们规定 $dp[i][isValid][j]$ 为前 $i$ 条边, 是否包含了一条边权至少为 $d$ 的边, 价值为 $j$ 的路径数量. 最后的答案就是 $\sum_{1\leq i\leq n}dp[i][1][n]$ . 枚举第 $i$ 条边的边权进行递推. 不难得到下面的状态转移方程:
$$
\begin{cases}
dp[i][0][j] = \sum_{1\leq x\leq j - 1} (I(x < d) \times dp[i - 1][0][j - x])\\
dp[i][1][j] = \sum_{1\leq x\leq j - 1} (dp[i - 1][1][j  - x] + I(x\geq d) × dp[i - 1][0][j - x]) \\
\end{cases}
$$
实际编写代码时, 我们利用刷表法来进行递推.
代码如下:
```C++
#include <bits/stdc++.h>
using namespace std;
const long long MOD = 1e9 + 7;
int main() {
    int n, k, d;
    cin>>n>>k>>d;
    vector<vector<vector<long long>>> dp(n + 1, vector<vector<long long>>(2, vector<long long>(n + 1, 0)));
    for (int j = 1; j <= min(k, n); j++) {
        if (j >= d) {
            dp[1][1][j] = 1;
        }
        else {
            dp[1][0][j] = 1;
        }
    }
    for (int i = 2; i <= n; i++) {
        for (int j = 1; j <= k; j++) { // 枚举第 i 条边价值为 j
            for (int t = 1; t <= n - j; t++) {
                if (j >= d) {
                    dp[i][1][t + j] += dp[i - 1][0][t] + dp[i - 1][1][t];
                    dp[i][1][t + j] %= MOD;
                }
                else {
                    dp[i][1][t + j] += dp[i - 1][1][t];
                    dp[i][0][t + j] += dp[i - 1][0][t];
                    dp[i][0][t + j] %= MOD;
                }
            }
        }
    }
    long long ans = 0;
    for (int i = 1; i <= n; i++) {
        ans += dp[i][1][n];
        ans %= MOD;
    }
    cout<<ans<<endl;
    return 0;
}
```

### Solution 2
状态的设计能否更精简一些? 实际上, 第一维 (边的个数) 是可以舍弃的. 直接根据后两个维度就可以递推了. 仔细观察我们的做法, 维度在状态转移时并没有起到实质性的作用. 这是因为我们最后得到的答案是遍历维度的和.  
记 $dp[i][isValid]$ 为价值为 $i$ , 是否包含了一条边权至少为 $d$ 的边的路径数量. 由如下递推关系:
$$
\begin{cases}
dp[i][0] = \sum_{1\leq j\leq i - 1}(I(j < d) \times dp[i - j][0])\\
dp[i][1] = \sum_{1\leq j\leq i - 1}(dp[i - j][1] + I(j \geq d)\times dp[i - j][0])
\end{cases}
$$
代码如下:
```C++
#include <bits/stdc++.h>
using namespace std;
const long long MOD = 1e9 + 7;
int main() {
    int n, k, d;
    cin>>n>>k>>d;
    vector<vector<long long>> dp(n + 1, vector<long long>(2, 0));
    dp[0][0] = 1;
    for (int i = 1; i <= n; i++) {
        for (int j = 1; j <= min(i, k); j++) {
            if (j < d) {
                dp[i][0] += dp[i - j][0];
                dp[i][0] %= MOD;
                dp[i][1] += dp[i - j][1];
                dp[i][1] %= MOD;
            }
            else {
                dp[i][1] += dp[i - j][0] + dp[i - j][1];
                dp[i][1] %= MOD;
            }
        }
    }
    cout<<dp[n][1]<<endl;
    return 0;
}
```

> 在计算 dp[i] 之前, dp[j] 已经计算完了, 不会出现混乱.