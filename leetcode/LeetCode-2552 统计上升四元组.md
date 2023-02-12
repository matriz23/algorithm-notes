---
title: LeetCode-2552 统计上升四元组 
date: 2023-02-09 23:11:00
updated:
tags: [计算机, 算法, LeetCode, C++]
categories: LeetCode题解
cover: 
description: LeetCode-2552「统计上升四元组」的思考与解答
---
### [LeetCode-2552 统计上升四元组](https://leetcode.cn/problems/count-increasing-quadruplets/)

### Solution 1
对于 $j < k$ , 需要知道的是 $i < j$ 且 $nums[i] < nums[k]$ 的 $i$ 的个数, 以及 $l > k$ 且 $nums[l] > nums[j]$ 的个数. 用二位前缀和 $f[i][j]$ 表示下标 $\leq i$ 且值 $\leq j$ 的元素个数. 对称地, 有 $g[i][j]$ . 最终答案即为
$$
\sum_{j<k,\ nums[j] > nums[k]}f[j - 1][nums[k] - 1]\times g[k + 1][nums[j] + 1]
$$
代码如下:
```C++
#define ll long long
ll f[4010][4010], g[4010][4010];
class Solution {
   public:
    long long countQuadruplets(vector<int>& nums) {
        int n = nums.size();
        ll ans = 0;
        for (int i = 0; i <= n + 1; i++) {
            for (int j = 0; j <= n + 1; j++) {
                f[i][j] = 0;
                g[i][j] = 0;
            }
        }
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < n; j++) {
                f[i][j] = ((i == 0) ? 0 : f[i - 1][j]) + (nums[i] <= j ? 1 : 0);
            }
        }
        for (int i = n - 1; i >= 0; i--) {
            for (int j = n; j >= 0; j--) {
                g[i][j] =
                    ((i == n - 1) ? 0 : g[i + 1][j]) + (nums[i] >= j ? 1 : 0);
            }
        }
        for (int i = 1; i < n; i++) {
            for (int j = i + 1; j < n - 1; j++) {
                if (nums[i] > nums[j]) {
                    ans += f[i - 1][nums[j] - 1] * g[j + 1][nums[i] + 1];
                }
            }
        }
        return ans;
    }
};
```