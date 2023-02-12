---
title: LeetCode-2358 最大价值和与最小价值和的差值 
date: 2023-01-17 11:19:00
updated:
tags: [计算机, 算法, LeetCode, C++, 图论]
categories: LeetCode题解
cover: 
description: LeetCode-2358「最大价值和与最小价值和的差值」的思考与解答
---
### [LeetCode-2358 最大价值和与最小价值和的差值](https://leetcode.cn/problems/difference-between-maximum-and-minimum-price-sum/)

### Solution 1
以 $u$ 为根的子树中, 从根节点出发, 不去掉叶子节点的最长路径为 $f[u]$ ; 从根节点出发, 去掉叶子节点的最长路径为 $g[u]$ . 则有:
$$\begin{cases}
f[u] = \underset{v\in e[u]}{max}(f[v] + price[u])\\
g[u] = \underset{v\in e[u]}{max}(g[v] + price[u])
\end{cases}
$$

用递归函数进行计算. 在考虑 $v$ 更新 $f[u], g[u]$ 之前, 以 $u$ 为根的子树中的最大价值和 $ans=max(ans, f[u] + g[v], f[v] + g[u])$ .
代码如下:
```C++
#define ll long long
typedef pair<ll, ll> pll;
class Solution {
public:
    long long maxOutput(int n, vector<vector<int>>& edges, vector<int>& price) {
        vector<int> e[n];
        for (auto &edge : edges) {
            e[edge[0]].push_back(edge[1]);
            e[edge[1]].push_back(edge[0]);
        }
        long long ans = 0;
        function<pll(int, int)> dp = [&](int sn, int fa) {
            long long f = price[sn], g = 0;
            for (int fn : e[sn]) if (fn != fa) {
                pll p = dp(fn, sn);
                long long ff = p.first, gg = p.second;
                ans = max({ans, f + gg, ff + g});
                f = max(f, ff + price[sn]); g = max(g, gg + price[sn]);
            }
            return pll(f, g);
        };
        dp(0, -1);
        return ans;
    }
};
```