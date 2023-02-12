---
title: LeetCode-2503 矩阵查询可获得的最大分数 
date: 2022-12-19 16:14:00
updated:
tags: [计算机, 算法, LeetCode, C++, 并查集]
categories: LeetCode题解
cover: 
description: LeetCode-2503「矩阵查询可获得的最大分数」的思考与解答
---
### [LeetCode-2503 矩阵查询可获得的最大分数](https://leetcode.cn/problems/maximum-number-of-points-from-grid-queries/)

### Solution 1
对于查询 $query[i]$ , 选出所有值 $< query[i]$ 的单元格, 所得分数就是左上角单元格 (如果 $grid[0][0] < query[i]$ )所在的最大联通块的大小. 
我们把 $query$ 数组从小到大排序, 同时用一个优先队列维护单元格, 用并查集维护连通块. 遍历 $query$ , 更新状态即可.
代码如下:
```C++
class Solution {
public:
    typedef pair<int, int> pii;
    const int dirs[4][2] = {{-1, 0}, {1, 0}, {0, -1}, {0, 1}};
    vector<int> pa;
    vector<int> sz;

    void Union(int x, int y) {
        x = Find(x);
        y = Find(y);
        if (x == y) {
            return;
        }
        else if (x < y) {
            pa[y] = x;
            sz[x] += sz[y];
        }
        else {
            pa[x] = pa[y];
            sz[y] += sz[x];
        }
    }

    int Find(int x) {
        if (x == pa[x]) {
            return x;
        }
        pa[x] = Find(pa[x]);
        return pa[x];
    }

    vector<int> maxPoints(vector<vector<int>>& grid, vector<int>& queries) {
        int m = grid.size();
        int n = grid[0].size();
        int k = queries.size();
        vector<int> ans(k, 0);
        pa = vector<int>(m * n + n, 0);
        iota(pa.begin(), pa.end(), 0);
        sz = vector<int>(m * n + n, 1);
        vector<pii> q_id(k);
        for (int i = 0; i < k; i++) {
            q_id[i] = make_pair(queries[i], i);
        }
        priority_queue<pii, vector<pii>, greater<pii>> q;
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                q.emplace(grid[i][j], i * n + j);
            }
        }
        sort(q_id.begin(), q_id.end());
        for (auto [bound, i]: q_id) {
            while (!q.empty() && q.top().first < bound) {
                auto [val, hash] = q.top();
                q.pop();
                int x = hash / n;
                int y = hash % n;
                for (auto dir: dirs) {
                    int nx = x + dir[0];
                    int ny = y + dir[1];
                    if (nx >= 0 && nx < m && ny >= 0 && ny < n && grid[nx][ny] < bound) {
                        Union(hash, nx * n + ny); 
                    }
                }
            }
            ans[i] = (grid[0][0] < bound? sz[0]: 0);
        }
        return ans;
    }
};
```