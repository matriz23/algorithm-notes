---
title: LeetCode-1377 T 秒后青蛙的位置 
date: 2022-11-13 22:12:00
updated:
tags: [计算机, 算法, LeetCode, C++]
categories: LeetCode题解
cover: 
description: LeetCode-1377「T 秒后青蛙的位置」的思考与解答
---
### [LeetCode-1377 T 秒后青蛙的位置](https://leetcode.cn/problems/frog-position-after-t-seconds/)

### Solution 1
由于本题青蛙在一棵无向树上跳动, 且不能往已跳过的点跳, 所以只可能从顶点不断向下跳. 记 $target$ 和根节点之间的距离为 $len$ , 则
- 如果 $len > t$ , 那么 $t$ 时间跳不到 $target$ , 概率为 $0$ ; 
- 如果 $len = t$ , 那么 $t$ 时间跳到的概率为路径上每一步概率之积;
- 如果 $len < t$ , 能否留在 $target$ 取决于其是不是叶子节点. 如果是, 那么概率同 $len = t$ 时, 否则为 $0$ .

我们可以用一次深度优先搜索得到每个点的父节点.
代码如下:
```C++
class Solution {
public:
    vector<int> pa;
    vector<vector<int>> e;
    void dfs(int p, int c) { // dfs, 得到每个点的父节点
        for (int i = 0; i < e[c].size(); i++) {
            if (e[c][i] == p) {
                continue;
            }
            pa[e[c][i]] = c;
            dfs(c, e[c][i]);
        }
    }

    double frogPosition(int n, vector<vector<int>>& edges, int t, int target) {
        pa = vector<int>(n + 1, 0);
        e = vector<vector<int>>(n + 1);
        e[1].push_back(0); // 为了统一, 我们设根节点的父节点为节点 0
        for (auto edge: edges) {
            e[edge[0]].push_back(edge[1]);
            e[edge[1]].push_back(edge[0]);
        }
        dfs(0, 1);
        int p = target;
        int len = 0;
        double x = 1.0;
        while (pa[p] != 0) {
            len++;
            p = pa[p];
            x *= (e[p].size() - 1);
        }
        return (len == t || (len < t && e[target].size() == 1))? 1.0 / x: 0;
    }
};
```