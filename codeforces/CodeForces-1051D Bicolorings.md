---
title: CodeForces-1051D Bicolorings 
date: 2022-07-19 15:57:00
updated:
tags: [计算机, 算法, CodeForces, C++, 动态规划]
categories: CodeForces题解
cover: https://raw.githubusercontent.com/wsylab/wsyPicGo/main/img/photo-1587888191477-e74ac6bc9c4b
description: CodeForces-1051D「Bicolorings」的思考与解答
---
### [CodeForces-1051D Bicolorings](https://codeforces.com/problemset/problem/1051/D)
### 题目大意
给定一个 $2\times n$ 的棋盘, 可以对上面的格子黑白染色. 求染色后棋盘上的联通块的个数正好为 $t$ 的染色方案数. 

### Solution 1
以 $0, 1$ 分别代表黑和白. 对于每一列, 我们有 $4$ 种染色方案, 分别为 $00$ , $01$ , $10$ , $11$ . 每次染色会带来列的数量和连通块数目的变化, 因此我们至少需要设定两个状态. 考虑具体的状态转移, 每次染色还与前一列末尾的染色情况关联, 因此我们需要设定一个三维的 $dp$ 数组, 其中 $dp[i][j][k]$ 为前 $i$ 列染色后构成 $j$ 个连通块, 且尾列状态为 $k$ 的染色方案数. 枚举可能的染色方法, 可以得到相应的状态转移方程.
代码如下:
```C++
#include <bits/stdc++.h>
using namespace std;
const int MOD = 998244353;
int main() {
    int n, t;
    cin>>n>>t;
    vector<vector<vector<long long>>> dp(n + 1, vector<vector<long long>>(max(t + 1, 3), vector<long long>(4, 0)));
    for (int k = 0; k <= 3; k++) {
        dp[1][1][k] = 1;
    }
    for (int i = 2; i <= n; i++) {
        for (int j = 1; j <= max(t, 2); j++) {
            for (int k = 0; k <= 3; k++) {
                int k_0 = k & 1;
                int k_1 = (k >> 1) & 1;
                for (int l = 0; l <= 3; l++) {
                    int l_0 = l & 1;
                    int l_1 = (l >> 1) & 1;
                    if (k_0 == k_1) {
                        if (k_0 == l_0 || k_0 == l_1) {
                            dp[i][j][k] += dp[i - 1][j][l];
                            dp[i][j][k] %= MOD;
                        }
                        else {
                            dp[i][j][k] += dp[i - 1][j - 1][l];
                            dp[i][j][k] %= MOD;
                        }
                    }
                    else {
                        if (l_0 == l_1) {
                            dp[i][j][k] += dp[i - 1][j - 1][l];
                            dp[i][j][k] %= MOD;
                        }
                        else if (k_0 == l_0) {
                            dp[i][j][k] += dp[i - 1][j][l];
                            dp[i][j][k] %= MOD;
                        }
                        else {
                            dp[i][j][k] += (j >= 2)? dp[i - 1][j - 2][l]: 0;
                            dp[i][j][k] %= MOD;
                        }
                    }
                }
            }
        }
    }
    long long ans = 0;
    for (int k = 0; k <= 3; k++) {
        ans += dp[n][t][k];
        ans %= MOD;
    }
    cout<<ans<<endl;
    return 0;
}
```

> - 我按照末尾状况再分类讨论的, 实际上把方程写好直接 dp 就可以了, 没有必要搞这么繁琐.

### Solution 2
实际上是对 Solution 1 的优化. 很多思想值得学习.
这里贴上 <font color=orange>0x3F</font> 的 AC 代码:
```Go
package main
import ."fmt"
func main() {
	const m = 998244353
	var n, k int
	Scan(&n, &k)
	if k == 1 {
		Print(2)
		return
	}
	f := make([][2]uint, k+1)
	f[1] = [2]uint{2, 0}
	f[2] = [2]uint{0, 2}
	for i := 2; i <= n; i++ {
		for j := min(k, i*2); j > 1; j-- {
			f[j][0] = (f[j][0] + f[j][1]*2 + f[j-1][0]) % m
			f[j][1] = (f[j][1] + f[j-1][0]*2 + f[j-2][1]) % m
		}
	}
	Print((f[k][0] + f[k][1]) % m)
}

func min(a, b int) int { if a > b { return b }; return a }
```

这份代码优化了什么? 
- 用滚动数组优化状态转移, 完成了降维
- 前 $j$ 列最多拥有 $2^k$ 个连通块, 可以减少不必要的枚举
- 以 $0$ 代表 $00, 11$ , 以 $1$ 代表 $01, 10$ , 利用对称性合并了状态.

不得不感叹, Coding 真是一门艺术.
  