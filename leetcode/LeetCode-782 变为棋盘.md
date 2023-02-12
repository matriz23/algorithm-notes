---
title: LeetCode-782 变为棋盘 
date: 2022-08-23 10:08:00
updated:
tags: [计算机, 算法, LeetCode, C++, 思维]
categories: LeetCode题解
cover: https://raw.githubusercontent.com/wsylab/wsyPicGo/main/img/photo-1632137366039-ba08c5ed24d3.avif
description: LeetCode-782「变为棋盘」的思考与解答
---
### [LeetCode-782 变为棋盘](https://leetcode.cn/problems/transform-to-chessboard/)

### Solution 1
这道题的要点在于看穿行列交换互不影响的特性. 我们首先假设初始局面能够通过行列交换变成一个 "棋盘" . 现在考虑每一行与第一行的关系. 由 "棋盘" 的定义可以知道, 每一行与第一行要么相同, 要么相反. 而交换两列不会影响行与行之间对应元素的相等性, 也就是说, 进行列交换的逆操作得到初始局面经过一系列行交换之后形成的局面, 在这个局面下, 每个行依然要么与第一行相同, 要么与第一行相反. 对列也是同理. 因此, 我们首先检查是否存在既不与第一行 / 列相同, 又不与第一行 / 列相反的行 / 列, 如果存在, 直接返回 `-1` ; 之后判断相同相反的个数是否能形成交错的局面; 最后把二维问题分解成两个一维问题, 计算最少的交换次数.
代码如下:
```C++
class Solution {
public:
    int movesToChessboard(vector<vector<int>>& board) {
        int n = board.size();
        int row_mask_0 = 0, row_mask_1 = 0, col_mask_0 = 0, col_mask_1 = 0;
        int row_cnt_0 = 1, col_cnt_0 = 1;
        for (int i = 0; i < n; i++) {
            row_mask_0 |= board[0][i] << i;
            row_mask_1 |= (1 - board[0][i]) << i;
            col_mask_0 |= board[i][0] << i;
            col_mask_1 |= (1 - board[i][0]) << i;
        }
        // 每一行要么是 row_mask_0, 要么是 row_mask_1
        for (int i = 1; i < n; i++) {
            int row_mask = 0;
            int col_mask = 0;
            for (int j = 0; j < n; j++) {
                row_mask |= board[i][j] << j;
                col_mask |= board[j][i] << j;
            }
            if ((row_mask != row_mask_0 && row_mask != row_mask_1) || (col_mask != col_mask_0 && col_mask != col_mask_1)) {
                return -1;
            }
            row_cnt_0 += (row_mask == row_mask_0);
            col_cnt_0 += (col_mask == col_mask_0);
        }
        if ((row_cnt_0 != n / 2 && row_cnt_0 != (n + 1) / 2) || (col_cnt_0 != n / 2 && col_cnt_0 != (n + 1) / 2)) { // 这一部分也可以通过 __builtin_popcount(mask) 判断
            return -1; 
        }
        // 下面考虑行交换和列交换的个数
        // 对于 0 1 1 0, 要交换几次?
        int ans = 0;
        int cnt[2];
        if (n % 2 == 0) { // n 为偶数时, 开头放 0 / 1 都行
            for (int start = 0; start <= 1; start++) {
                cnt[start] = 0;
                for (int i = 0; i < n; i += 2) {
                    cnt[start] += (board[0][i] != start);
                }
            }
            ans += min(cnt[0], cnt[1]);
            for (int start = 0; start <= 1; start++) {
                cnt[start] = 0;
                for (int i = 0; i < n; i += 2) {
                    cnt[start] += (board[i][0] != start);
                }
            }
            ans += min(cnt[0], cnt[1]);
        }
        else { // n 为奇数时, 开头只能放数量更多的那个元素
            int row_ones = __builtin_popcount(row_mask_0), col_ones = __builtin_popcount(col_mask_0);
            int row_start = (row_ones == n / 2)? 0: 1;
            int col_start = (col_ones == n / 2)? 0: 1;
            for (int i = 0; i < n; i += 2) {
                ans += (board[0][i] != row_start);
                ans += (board[i][0] != col_start);
            }
        }
        return ans;
    }
};
```