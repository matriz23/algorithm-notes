---
title: LeetCode-793 阶乘函数后 K 个零 
date: 2022-08-28 08:40:00
updated:
tags: [计算机, 算法, LeetCode, C++, 数学, 二分]
categories: LeetCode题解
cover: https://raw.githubusercontent.com/wsylab/wsyPicGo/main/img/photo-1497996377197-e4b9024658a4.jpg
description: LeetCode-793「阶乘函数后 K 个零」的思考与解答
---
### [LeetCode-793 阶乘函数后 K 个零](https://leetcode.cn/problems/preimage-size-of-factorial-zeroes-function/)

### Solution 1
对于 $x!$ , 其中 $2$ 的因子一定不少于 $5$ 的因子, 因此 $k = x!$ 中因子 $5$ 的数量, 记为 $zeta(x)$ . 对于 $zeta(x)$ , 分别考虑 $1, 2, ..., x$ 中有 $1, 2, ...$ 个因子 $5$ 的数的个数, 则 $zeta(x) = \lfloor\frac{x}{5}\rfloor + \lfloor\frac{x}{5^2}\rfloor + ...$ . 因为 $zeta(x)$ 具有单调性, 我们二分搜索是否存在这样的 $x$ , 使得 $zeta(x) = k$ . 如果存在, 那么根据 $zeta(x)$ 的特征, 答案一定是 $5$ ; 否则答案是 $0$ .
代码如下:
```C++
#define ll long long
class Solution {
public:
    int zeta(long long x) {
        int res = 0;
        while (x / 5 != 0) {
            res += x / 5;
            x /= 5;
        }
        return res;
    }

    int preimageSizeFZF(int k) {
        long long left = 0;
        long long right = 1e10;
        while (left <= right) {
            long long mid = left + (right - left) / 2;
            if (zeta(mid) < k) {
                left = mid + 1;
            }
            else if (zeta(mid) > k) {
                right = mid - 1;
            }
            else {
                return 5;
            }
        }
        return 0;
    }
};
```