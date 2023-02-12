---
title: LeetCode-2543 判断一个点是否可以到达 
date: 2023-01-29 09:49:00
updated:
tags: [计算机, 算法, LeetCode, C++, 数论]
categories: LeetCode题解
cover: 
description: LeetCode-2543「判断一个点是否可以到达」的思考与解答
---
### [LeetCode-2543 判断一个点是否可以到达](https://leetcode.cn/problems/check-if-point-is-reachable/)

### Solution 1
逆向考虑, 从 $(targetX, targetY)$ 到达 $(1, 1)$ , 每次操作可以从 $(x, y)$ 变成:
- $(x, x + y)$
- $(x + y, x)$
- $(x / 2, y)$, 如果 $x$ 为偶数
- $(x, y / 2)$, 如果 $y$ 为偶数

如果后两种可行, 那么优先使用这两种操作. 因为更小尺度上的操作同样能够完成更大尺度上的操作. 此外, 前两种操作不会影响 $GCD(x, y)$ , 类似辗转相除, 可以依据这一点判定是否可以到达 $(1, 1)$ .

代码如下:
```C++
class Solution {
public:
    bool isReachable(int targetX, int targetY) {
        int res = __gcd(targetX, targetY);
        return (res & (res - 1)) == 0;
    }
};
```