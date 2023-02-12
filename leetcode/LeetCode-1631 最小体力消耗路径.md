---
title: LeetCode-1631 最小体力消耗路径
date: 2022-08-31 10:05:00
updated:
tags: [计算机, 算法, LeetCode, C++, 二分, 搜索, 并查集, 最短路]
categories: LeetCode题解
cover: https://raw.githubusercontent.com/wsylab/wsyPicGo/main/img/photo-1507715248392-5b5da04f28d2
description: LeetCode-1631「最小体力消耗路径」的思考与解答
---
### [LeetCode-1631 最小体力消耗路径](https://leetcode.cn/problems/path-with-minimum-effort/)

### Solution 1
如果最终答案是 $ans$ , 那么 $ans$ 所在路径中所有相邻格子的高度差的绝对值都不应该超过 $ans$ . 因此, 我们可以枚举 $ans$ 的可能大小, 通过广度优先搜索寻找一条到 $(m - 1, n - 1)$ 的路径. 如果路径存在, 那么尝试寻找更小的 $ans$ ; 如果路径不存在, 尝试寻找更大的 $ans$ . 这个过程可以使用二分搜索进一步优化.
代码如下:
```C++
typedef pair<int, int> pii;
class Solution {
public:
    const int dirs[4][2] = {{-1, 0}, {1, 0}, {0, -1}, {0, 1}};
    int minimumEffortPath(vector<vector<int>>& heights) {
        int m = heights.size();
        int n = heights[0].size();
        int left = 0;
        int right = 1e6;
        int ans = 0;
        while (left <= right) {
            int mid = left + (right - left) / 2;
            vector<vector<bool>> visited(m, vector<bool>(n, false));
            queue<pii> q;
            q.emplace(0, 0);
            visited[0][0] = true;
            while (!q.empty()) {
                auto [x, y] = q.front();
                q.pop();
                for (auto dir: dirs) {
                    int nx = x + dir[0];
                    int ny = y + dir[1];
                    if (nx >= 0 && nx < m && ny >= 0 && ny < n && abs(heights[nx][ny] - heights[x][y]) <= mid && !visited[nx][ny]) {
                        q.emplace(nx, ny);
                        visited[nx][ny] = true;
                    }
                }
            } 
            if (visited[m - 1][n - 1]) {
                ans = mid;
                right = mid - 1;
            }
            else {
                left = mid + 1;
            }            
        }
        return ans;
    }
};
```

### Solution 2
类似 Solution 1 的思考方式, 一条路径由满足共同性质的一些块构成, 不断放宽限制 (即增大寻找的 $ans$ ), 联通部分会越来越多, 直到形成一条合法路径为止. 这实际上就是一个并查集的模型. 不过这里的块不是点, 而是边 (显然单独一个点是没有什么性质的) .
代码如下:
```C++
class UnionFind {
public:
    vector<int> parent;
    vector<int> size;
    int n;
    // 当前连通分量数目
    int setCount;
    
public:
    UnionFind(int _n): n(_n), setCount(_n), parent(_n), size(_n, 1) {
        iota(parent.begin(), parent.end(), 0);
    }
    
    int findset(int x) {
        return parent[x] == x ? x : parent[x] = findset(parent[x]);
    }
    
    bool unite(int x, int y) {
        x = findset(x);
        y = findset(y);
        if (x == y) {
            return false;
        }
        if (size[x] < size[y]) {
            swap(x, y);
        }
        parent[y] = x;
        size[x] += size[y];
        --setCount;
        return true;
    }
    
    bool connected(int x, int y) {
        x = findset(x);
        y = findset(y);
        return x == y;
    }
};

class Solution {
public:
    int minimumEffortPath(vector<vector<int>>& heights) {
        int m = heights.size();
        int n = heights[0].size();
        vector<tuple<int, int, int>> edges;
        for (int i = 0; i < m; ++i) {
            for (int j = 0; j < n; ++j) {
                int id = i * n + j;
                if (i > 0) {
                    edges.emplace_back(id - n, id, abs(heights[i][j] - heights[i - 1][j]));
                }
                if (j > 0) {
                    edges.emplace_back(id - 1, id, abs(heights[i][j] - heights[i][j - 1]));
                }
            }
        }
        sort(edges.begin(), edges.end(), [](const auto& e1, const auto& e2) {
            auto&& [x1, y1, v1] = e1;
            auto&& [x2, y2, v2] = e2;
            return v1 < v2;
        });

        UnionFind uf(m * n);
        int ans = 0;
        for (const auto [x, y, v]: edges) {
            uf.unite(x, y);
            if (uf.connected(0, m * n - 1)) {
                ans = v;
                break;
            }
        }
        return ans;
    }
};
```

> Solution 1 和 Solution 2 最有趣的地方在于, 不是寻找路径的过程中更新 "距离" (本题的距离很特殊, 实际上是由各个边自身的性质约束的) , 而是限定了约束再看能否找到一个合法路径.

### Solution 3
优先队列优化的 Dijkstra 算法.
代码如下:
```C++
class Solution {
private:
    static constexpr int dirs[4][2] = {{-1, 0}, {1, 0}, {0, -1}, {0, 1}};
    
public:
    int minimumEffortPath(vector<vector<int>>& heights) {
        int m = heights.size();
        int n = heights[0].size();
        
        auto tupleCmp = [](const auto& e1, const auto& e2) {
            auto&& [x1, y1, d1] = e1;
            auto&& [x2, y2, d2] = e2;
            return d1 > d2;
        };
        priority_queue<tuple<int, int, int>, vector<tuple<int, int, int>>, decltype(tupleCmp)> q(tupleCmp);
        q.emplace(0, 0, 0);

        vector<int> dist(m * n, INT_MAX);
        dist[0] = 0;
        vector<int> seen(m * n);

        while (!q.empty()) {
            auto [x, y, d] = q.top();
            q.pop();
            int id = x * n + y;
            if (seen[id]) {
                continue;
            }
            if (x == m - 1 && y == n - 1) {
                break;
            }
            seen[id] = 1;
            for (int i = 0; i < 4; ++i) {
                int nx = x + dirs[i][0];
                int ny = y + dirs[i][1];
                if (nx >= 0 && nx < m && ny >= 0 && ny < n && max(d, abs(heights[x][y] - heights[nx][ny])) < dist[nx * n + ny]) {
                    dist[nx * n + ny] = max(d, abs(heights[x][y] - heights[nx][ny]));
                    q.emplace(nx, ny, dist[nx * n + ny]);
                }
            }
        }  
        return dist[m * n - 1];
    }
};
```