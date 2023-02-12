---
title: LeetCode-1760 袋子里最少数目的球 
date: 2022-12-20 11:23:00
updated:
tags: [计算机, 算法, LeetCode, Python, 二分]
categories: LeetCode题解
cover: 
description: LeetCode-1760「袋子里最少数目的球」的思考与解答
---
### [LeetCode-1760 袋子里最少数目的球](https://leetcode.cn/problems/minimum-limit-of-balls-in-a-bag/)

### Solution 1
如果 $k$ 开销可以, 那么 $k + 1$ 的开销也可以. 同时我们可以在 $O(n)$ 时间内验证一个开销是否能够达成. 因此用二分搜索最小开销即可. 
代码如下:
```Python
class Solution:
    def minimumSize(self, nums: List[int], maxOperations: int) -> int:
        left = 1
        right = max(nums)
        while left < right:
            mid = left + (right - left) // 2
            def check(nums: List[int], maxOperations: int, k: int) -> bool:
                for v in nums:
                    maxOperations -= (v - 1) // k
                return maxOperations >= 0
            if check(nums, maxOperations, mid):
                right = mid
            else:
                left = mid + 1
        return left
```