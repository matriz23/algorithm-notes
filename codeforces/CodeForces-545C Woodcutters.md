---
title: CodeForces-545C Woodcutters 
date: 2022-06-23 02:53:00
updated:
tags: [计算机, 算法, CodeForces, C++]
categories: CodeForces题解
cover: https://raw.githubusercontent.com/wsylab/wsyPicGo/main/img/photo-1566744883356-fb704591844e
description: CodeForces-545C「Woodcutters」的思考与解答
---
### [CodeForces-545C Woodcutters](https://codeforces.com/problemset/problem/545/C)

### 题目大意
输入正整数 $n$ 与形状为 $n × 2$ 的数组 $tree$ , 其中 $tree[i][0]$ 代表第 $i$ 棵树在数轴上的坐标, $tree[i][1]$ 代表第 $i$ 棵树的高度. 伐木时需要让树向左倒或向右倒, 要求空区间长度大于树木高度. 返回最多能砍多少棵树.
### Solution 1
To do or not to do问题, 可以用动态规划进行求解. 对于一棵树, 我们可以选择不砍, 向左倒或者向右倒, 依据这一特点进行状态转移方程的构建. 对于前 $i$ 棵树, 记 $dp[i][0]$ 不砍第 $i$ 棵树最多砍伐的数目, 类似的, $dp[i][1]$ 为向左, $dp[i][2]$ 为向右. 当不砍伐当前树木时, 显然有:
$$
dp[i][0] = max(dp[i - 1][0], dp[i][1], dp[i][2])
$$
如果选择向左倒, 如果左侧区间足够容纳两棵树, 则在前一棵树右倒的情况下左倒; 如果仅能容纳第 $i$ 棵树, 则在前一棵树不砍或左倒的情况下左倒; 如果不能容纳第 $i$ 棵树, 则该操作不合法, 数量为 $0$ . 则有:
$$
dp[i][1] = 
\begin{cases}
dp[i - 1][2] + 1,\quad tree[i][0] - tree[i - 1][0] > tree[i][1] + tree[i - 1][1]\\
max(dp[i - 1][0], dp[i - 1][1]) + 1,\quad tree[i][1] + tree[i - 1][1]\geq tree[i][0] - tree[i - 1][0] > tree[i][1]\\
0,\quad tree[i][0] - tree[i - 1][0] \leq tree[i][1]
\end{cases}
$$
如果选择向右倒, 则考虑右侧区间是否能够容纳第 $i$ 棵树, 状态转移方程如下:
$$
dp[i][2] =
\begin{cases}
max(dp[i - 1][0], dp[i - 1][1], dp[i - 1][2]) + 1),\quad tree[i + 1][0] - tree[i][0]) > tree[i][1]\\
0,\quad tree[i + 1][0] - tree[i][0]) \leq tree[i][1]
\end{cases}
$$
最终结果即为 $max(dp[n - 1][0, dp[]n - 1][1], dp[n - 1][2])$ , 注意到最后一颗树向右倒总是最优的, 所以结果其实就是 $dp[n - 1][2]$.
代码如下:
```C++
#include <bits/stdc++.h>
using namespace std;
int main() {
    int n;
    cin>>n;
    vector<vector<long long>> tree(n + 1, vector<long long>(2, 0));
    for (int i = 0; i < n; i++) {
        cin>>tree[i][0]>>tree[i][1];
    }
    tree[n][0] = 1000000000 * 2 + 10; // 加一棵位置很远的虚拟树, 方便i = n - 1时的处理
    // 从左向右考虑tree, tree[i]是否砍倒
    vector<vector<int>> dp(n, vector<int>(3, 0)); // dp[i][0]为tree[i]不砍, 1为向左倒, 2为向右倒
    dp[0][0] = 0;
    dp[0][1] = 1;
    dp[0][2] = (tree[1][0] - tree[0][0]) > tree[0][1];
    for (int i = 1; i < n; i++) {
        dp[i][0] = max(max(dp[i - 1][0], dp[i - 1][1]), dp[i - 1][2]);
        if (tree[i][0] - tree[i - 1][0] > tree[i][1] + tree[i - 1][1]) {
            dp[i][1] = dp[i - 1][2] + 1;
        }
        else if (tree[i][0] - tree[i - 1][0] > tree[i][1]) {
            dp[i][1] = max(dp[i - 1][0], dp[i - 1][1]) + 1;
        }
        dp[i][2] = (max(max(dp[i - 1][0], dp[i - 1][1]), dp[i - 1][2]) + 1) * ((tree[i + 1][0] - tree[i][0]) > tree[i][1]);
    }
    cout<<dp[n - 1][2]<<endl; // 最后一棵树向右倒总是最优的
    return 0;
}
```

### Solution 2
本题也可以用贪心的思想求解. 对于一棵树, 如果能向左倒一定选择向左倒, 如果不能向左倒呢? 其实这时向右倒是最优的. 不严谨的证明如下: 不向右倒带来的缺憾(数量 $- 1$)至多被后面的树左倒补足, 所以我们这时选择右倒已经是最优解.
代码如下:
```C++
#include <bits/stdc++.h>
using namespace std;
int main() {
    int n;
    cin>>n;
    vector<vector<long long>> tree(n + 1, vector<long long>(2, 0));
    for (int i = 0; i < n; i++) {
        cin>>tree[i][0]>>tree[i][1];
    }
    tree[n][0] = 1000000000 * 2 + 10;
    int ans = 1;
    for (int i = 1; i < n; i++) {
        if (tree[i][0] - tree[i - 1][0] > tree[i][1]) {
            ans++;
        }
        else if (tree[i + 1][0] - tree[i][0] > tree[i][1]) {
            ans++;
            tree[i][0] = tree[i][0] + tree[i][1];
        }
    }
    cout<<ans<<endl;
    return 0;
}
```