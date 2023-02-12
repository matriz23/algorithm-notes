---
title: CodeForces-377A Maze 
date: 2022-07-21 16:42:00
updated:
tags: [计算机, 算法, CodeForces, C++, 深度优先搜索]
categories: CodeForces题解
cover: https://raw.githubusercontent.com/wsylab/wsyPicGo/main/img/photo-1574390353491-92705370c72e
description: CodeForces-377A「Maze」的思考与解答
---
### [CodeForces-377A Maze](https://codeforces.com/problemset/problem/377/A)
### 题目大意
给定一个由空地 $.$ 与 障碍 $\#$ 构成的地图, 其中所有的空地都四联通. 要求把 $k$ 个空地变成墙 $X$ , 使得剩下的空地仍然联通. 返回改变后的地图.
### Solution 1
为了不破坏空地的连通性, 我们需要尽可能先从联通块的 "外层" 进行改变. 一个可行的思路是从任意一个空地开始, 利用深度优先搜索并存储点, 再倒着改变. 不过一种更巧妙的思路是, 我们先把空地都变成墙, 再利用深度优先搜索开拓出一个连通块, 扩展连通块明显更容易实现.
代码如下:
```C++
#include <bits/stdc++.h>
using namespace std;
const int dirs[4][2] = {{1,0},{0,-1},{0,1},{-1,0}};

void dfs (int x, int y, int &k, vector<vector<char>> &grid) {
    if (x < 0 || x >= grid.size() || y < 0 || y >= grid[0].size()) {
        return;
    }
    if (grid[x][y] != 'X') {
        return;
    }
    if (k == 0) {
        return;
    }
    grid[x][y] = '.';
    k--;
    for (auto dir: dirs) {
        dfs(x + dir[0], y + dir[1], k, grid);
    }
    return;
}

int main() {
    int n, m, k;
    cin>>n>>m>>k;
    vector<vector<char>> grid(n, vector<char>(m, '#'));
    int x, y;
    int cnt = 0;
    for (int i = 0; i < n; i++) {
        for (int j = 0; j < m; j++) {
            cin>>grid[i][j];
            if (grid[i][j] == '.') {
                grid[i][j] = 'X';
                x = i;
                y = j;
                cnt++;
            }
        }
    }
    k = cnt - k;
    dfs(x, y, k, grid);
    for (int i = 0; i < n; i++) {
        for (int j = 0; j < m; j++) {
            cout<<grid[i][j];
        }
        cout<<endl;
    }
    return 0;
}
```