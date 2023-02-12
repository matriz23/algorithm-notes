---
title: LeetCode-1444 切披萨的方案数 
date: 2022-11-14 16:04:00
updated:
tags: [计算机, 算法, LeetCode, Python, 动态规划]
categories: LeetCode题解
cover: 
description: LeetCode-1444「切披萨的方案数」的思考与解答
---
### [LeetCode-1444 切披萨的方案数](https://leetcode.cn/problems/number-of-ways-of-cutting-a-pizza/)

### Solution 1
注意切割性质: 水平切一刀留下下半部分, 竖直切一刀留下右半部分, 因此任何时候我们处理的披萨都是左上端点为 $(i, j)$ , 右下端点为 $(m - 1, n - 1)$ 的部分. 考虑 $f(i, j, k)$ 代表左上端点为 $(i, j)$ , 右下端点为 $(m - 1, n - 1)$, 需要切成 $k$ 块的方案数量. 每次枚举可能的切割点.
对于判断是否有苹果, 可以用二维前缀和预处理. 
代码如下:
```Python
class Solution:
    def ways(self, pizza: List[str], k: int) -> int:
        MOD = 1e9 + 7
        m = len(pizza)
        n = len(pizza[0])
        cnt = [[0]*n for _ in range(m)]
        cnt[m - 1][n - 1] = (int)(pizza[m - 1][n - 1] == 'A')
        for i in range(m - 2, -1, -1):
            cnt[i][n - 1] = cnt[i + 1][n - 1] + (pizza[i][n - 1] == 'A')
        for j in range(n - 2, -1, -1):
            cnt[m - 1][j] = cnt[m - 1][j + 1] + (pizza[m - 1][j] == 'A')
        for i in range(m - 2, -1, -1):
            for j in range(n - 2, -1, -1):
                cnt[i][j] = cnt[i][j + 1] + cnt[i + 1][j] + (pizza[i][j] == 'A') - cnt[i + 1][j + 1]
        @cache
        def f(i: int, j: int, k: int) -> int:
            if k == 1:
                if cnt[i][j] > 0: 
                    return 1
                return 0
            res = 0
            for x in range(i + 1, m):
                if cnt[i][j] - cnt[x][j] > 0:
                    if cnt[x][j] >= k - 1:
                        res = (res + f(x, j, k - 1)) % MOD
                    else:
                        break
            for y in range(j + 1, n):
                if cnt[i][j] - cnt[i][y] > 0:
                    if cnt[i][y] >= k - 1:
                        res = (res + f(i, y, k - 1)) % MOD
                    else:
                        break
            return int(res)
        return f(0, 0, k)
```