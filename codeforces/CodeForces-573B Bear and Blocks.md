---
title: CodeForces-573B Bear and Blocks 
date: 2022-07-18 17:04:00
updated:
tags: [计算机, 算法, CodeForces, C++, 动态规划, 思维]
categories: CodeForces题解
cover: https://raw.githubusercontent.com/wsylab/wsyPicGo/main/img/photo-1471679984494-b1491dff4144
description: CodeForces-573B「Bear and Blocks」的思考与解答
---
### [CodeForces-573B Bear and Blocks](https://codeforces.com/problemset/problem/573/B)
### 题目大意
有连续 $n$ 列柱子, 第 $i$ 列由 $h_i$ 个小方块堆成. 每次操作可以消去所有与外界接触的小方块, 求消除所有柱子所需要的次数.
### Solution 1
一个柱子被消掉有两种方法: 一种左/右侧的柱子已经消去, 则这根柱子会被一次全部消去; 另一种是柱子每次都消去最上方的小方块直至完全消失. 我们记 $dp[i]$ 为第 $i$ 根柱子完全消失所用的次数.
先考虑第一种情况, 以左侧为例, 有 $dp[i] = dp[i - 1] + 1$ .
再仔细思考一下第二种情况. 如果柱子低于左右两侧的柱子, 那么每次都会消去一个小方块, 否则会一次性抹平到左右侧最低的柱子, 再慢慢消去顶端的小方块. 自己慢慢消去需要 $h_i$ 次, 而通过周围柱子消去(这里以左侧柱子为例), 则是 $h_{i - 1} + 1$ 次. 事实上不必再考虑 $h_{i - 1} + 1$ 以及之后的迭代. 对于每根柱子, 我们都要比较 $dp[i - 1] + 1$ 与 $h_i$ . 在考虑第 $i$ 根柱子时, $dp[i - 1] + 1$ 实际上已经同时继承了两种消去方式, 我们只需要和 $h_i$ 比较即可. 最终状态转移方程如下:
$$
dp[i] = max(dp[i - 1] + 1, dp[i + 1] + 1, h_i)
$$
具体实现时, 我们可以添加两根高度为 $0$ 的虚拟柱子, 从左向右遍历更新一次 $dp$ , 再从右向左遍历更新一次 $dp$ . 最终 $\underset{1
\leq i\leq n}{max}\ dp[i]$ 就是所求次数.
代码如下:
```C++
#include <bits/stdc++.h>
using namespace std;
int main() {
    int n;
    cin>>n;
    int ans = 0;
    vector<int> h(n + 2, 0);
    vector<int> dp(n + 2, 0);
    for (int i = 1; i <= n; i++) {
        cin>>h[i];
    }
    for (int i = 1; i <= n; i++) {
        dp[i] = min(dp[i - 1] + 1, h[i]);
    }
    for (int i = n; i >= 1; i--) {
        dp[i] = min(dp[i], min(dp[i + 1] + 1, h[i]));
        ans = max(ans, dp[i]);
    }
    cout<<ans<<endl;
    return 0;
}
```