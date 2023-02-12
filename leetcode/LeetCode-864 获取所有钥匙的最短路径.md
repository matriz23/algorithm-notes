---
title: LeetCode-864 获取所有钥匙的最短路径 
date: 2022-11-13 20:22:00
updated:
tags: [计算机, 算法, LeetCode, C++]
categories: LeetCode题解
cover: 
description: LeetCode-864「获取所有钥匙的最短路径」的思考与解答
---
### [LeetCode-864 获取所有钥匙的最短路径](https://leetcode.cn/problems/shortest-path-to-get-all-keys/)

### Solution 1
本题中, 任一时刻的状态有三个分量: 横坐标 $x$ , 纵坐标 $y$ 与当前已有钥匙集合 $s$ . 在这三个状态为顶点构成的图上进行广度优先搜索即可. 
代码如下:
```C++
class Solution {
public:
    typedef pair<int, int> pii;
    typedef tuple<int, int, int> tiii;
    const int dirs[4][2] = {{1, 0}, {-1, 0}, {0, 1}, {0, -1}};
    vector<vector<vector<bool>>> visited;

    int shortestPathAllKeys(vector<string>& grid) {
        int m = grid.size();
        int n = grid[0].size();
        int k = 0;
        tiii start;
        map<char, pii> keys; // 存储钥匙的坐标
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (grid[i][j] == '@') {
                    start = make_tuple(i, j, 0);
                    grid[i][j] = '.'; // 起点和空地没区别, 修改以统一
                }
                else if (grid[i][j] >= 'a' && grid[i][j] <= 'z') {
                    k++;
                    keys[grid[i][j]] = make_pair(i, j);
                }
            }
        }
        visited = vector<vector<vector<bool>>> (m, vector<vector<bool>>(n, vector<bool>(1 << k, false))); 
        int ans = 0;
        queue<tiii> q;
        q.push(start);
        while (!q.empty()) {
            ans++;
            int sz = q.size();
            while (sz--) {
                auto [x, y, s] = q.front();
                q.pop();
                for (auto dir: dirs) {
                    int nx = x + dir[0];
                    int ny = y + dir[1];
                    int ns = s;
                    if (nx < 0 || nx >= m || ny < 0 || ny >= n || grid[nx][ny] == '#' || visited[nx][ny][s]) { // 相邻点越界或是障碍的情况, 直接跳过
                        continue;
                    }
                    else if (grid[nx][ny] >= 'A' && grid[nx][ny] <= 'Z') { // 锁, 需要对应的钥匙才能打开
                        if (!(s & 1 << (int)(grid[nx][ny] - 'A'))) {
                            continue;
                        }
                    }
                    else if (grid[nx][ny] >= 'a' && grid[nx][ny] <= 'z') { // 钥匙, 更新 s
                        ns = s | 1 << (int)(grid[nx][ny] - 'a');
                        if (ns == (1 << k) - 1) {
                            return ans;
                        }
                    }
                    q.emplace(nx, ny, ns);
                    visited[nx][ny][ns] = true;
                }
            }
        }   
        return -1;
    }
};
```