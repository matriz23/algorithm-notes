---
title: LeetCode-2397 被列覆盖的最多行数 
date: 2022-09-12 00:19:00
updated:
tags: [计算机, 算法, LeetCode, C++]
categories: LeetCode题解
cover: https://raw.githubusercontent.com/wsylab/wsyPicGo/main/img/photo-1662574428878-6969eba0b86c.avif
description: LeetCode-2397「被列覆盖的最多行数」的思考与解答
---
### [LeetCode-2397 被列覆盖的最多行数](https://leetcode.cn/problems/maximum-rows-covered-by-columns/)

### Solution 1
数据范围很小, 我们枚举选择列的情况, 用一个数的二进制来表示选择了哪些列, 不断更新 $ans$ .
代码如下:
```C++
class Solution {
public:
    int maximumRows(vector<vector<int>>& mat, int cols) {
        int m = mat.size();
        int n = mat[0].size(); // 从 n 行里取出cols
        int ans = 0;
        for (int i = (1 << (n + 1)) - 1; i >= 1; i--) { // 至多选择 n 列, 至少选择 1 列
            if (__builtin_popcount(i) != cols) {
                continue; // 枚举的列数不为 cols 直接跳过
            }
            set<int> book; // 记录选择了哪些列
            for (int j = 0; j < n; j++) {
                if (i & (1 << j)) {
                    book.insert(j); 
                }
            }
            int cnt = 0;
            for (int x = 0; x < m; x++) { // 遍历行, 统计被覆盖的行数
                bool isValid = true;
                for (int y = 0; y < n && isValid; y++) {
                    if (mat[x][y] == 1) {
                        if (!book.count(y)) {
                            isValid = false;
                        }
                    }
                }
                if (isValid) {
                    cnt++;
                }
            }
            ans = max(ans, cnt);
        }
        return ans;
    }
};
```