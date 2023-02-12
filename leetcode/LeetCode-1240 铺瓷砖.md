---
title: LeetCode-1240 铺瓷砖 
date: 2022-08-02 09:09:00
updated:
tags: [计算机, 算法, LeetCode, C++, 递归, 回溯]
categories: LeetCode题解
cover: https://raw.githubusercontent.com/wsylab/wsyPicGo/main/img/photo-1622363405079-da46534ce5ec
description: LeetCode-1240「铺瓷砖」的思考与解答
---
### [LeetCode-1240 铺瓷砖](https://leetcode.cn/problems/tiling-a-rectangle-with-the-fewest-squares/)

### Solution 1
很标准的回溯算法, 具体的细节见代码注释.
代码如下:
```C++
class Solution {
public:
    int grid[13][13]; // 0 代表没铺瓷砖, 1 代表铺了瓷砖
    int ans = 13 * 13; // 全用 1 × 1 的瓷砖, 答案不超过 13 × 13

    void dfs(int n, int m, int cur) { // 递归函数, n × m 的地板, 现在用了 cur 块瓷砖
        if (cur >= ans) { // 如果当前所用瓷砖已经超过目前最优解, 不会得到更优解了, 跳过
            return;
        }
        bool isFull = true; // 记录当前地板是否已经铺满
        int row, col; // 记录第一个空格的坐标
        for (int i = 0; i < n && isFull; i++) {
            for (int j = 0; j < m && isFull; j++) {
                if (grid[i][j] == 0) {
                    isFull = false;
                    row = i;
                    col = j;
                }
            }
        }
        if (isFull) { // 如果铺满了, 更新答案
            ans = cur;
            return;
        }
        // 如果没有铺满, 从大到小开始试瓷砖
        for (int len = min(n - row, m - col); len >=1; len--) {
            bool isEmpty = true; // 判断 len 大小的瓷砖占用的地方是否是空地
            for (int i = row; i < row + len && isEmpty; i++) {
                for (int j = col; j < col + len && isEmpty; j++) {
                    if (grid[i][j] == 1) {
                        isEmpty = false;
                    }
                }
            }
            if (!isEmpty) { // 如果不是空地, 说明 len 太大, 到循环的下一步试更小的瓷砖
                continue;
            }
            else { // 如果是空地, 说明 len 可用, 继续递归, 先把当前的铺满
                for (int i = row; i < row + len; i++) {
                    for (int j = col; j < col + len; j++) {
                        grid[i][j] = 1;
                    }
                }
                dfs(n, m, cur + 1); // 递归下一步, 所用瓷砖数量 + 1
                // 回溯, 把这一步铺的瓷砖撤销, 不影响循环的下一步
                for (int i = row; i < row + len; i++) {
                    for (int j = col; j < col + len; j++) {
                        grid[i][j] = 0;
                    }
                }
            }
        }
    }

    int tilingRectangle(int n, int m) {
        if (n == m) { // 简单情况直接返回答案
            return 1;
        }
        memset(grid, 0, sizeof(grid)); // 初始化空地
        dfs(n, m, 0); // 开始递归
        return ans;
    }
};
```

### 补充
关于这道题目, [LeetCode 的题解区](https://leetcode.cn/problems/tiling-a-rectangle-with-the-fewest-squares/solution/) 有不少文章都讲解了动态规划的算法 (复杂度$O(n^4)$) , 这些算法能通过本题数据 $(n, m \leq 13)$ , 但在更大的数据下存在反例. 本问题事实上是 NP 完全问题, 不存在多项式时间内的解法, 上文所用的 dfs 回溯才是正确的解法.