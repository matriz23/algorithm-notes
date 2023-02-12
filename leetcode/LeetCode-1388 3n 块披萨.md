---
title: LeetCode-1388 3n 块披萨 
date: 2022-10-03 22:30:00
updated:
tags: [计算机, 算法, LeetCode, C++, 动态规划]
categories: LeetCode题解
cover: https://raw.githubusercontent.com/wsylab/wsyPicGo/main/img/photo-1476362174823-3a23f4aa6d76
description: LeetCode-1388「3n 块披萨」的思考与解答
---
### [LeetCode-1388 3n 块披萨](https://leetcode.cn/problems/pizza-with-3n-slices/)

### Solution 1
取完某块披萨, 相邻的披萨一定不能再取了, 因此取的元素至少满足不能相邻这一点. 实际上, 从 $3n$ 个元素中取出 $n$ 个不相邻的元素一定能够对应某种披萨的取法. 详细的证明可以参考[力扣官方题解](https://leetcode.cn/problems/pizza-with-3n-slices/solution/3n-kuai-pi-sa-by-leetcode-solution/). 知晓这一点后利用动态规划即可.
代码如下:
```C++
class Solution {
public:
    int maxSizeSlices(vector<int>& slices) {
        int n = slices.size() / 3;
        vector<vector<int>> dp(3 * n, vector<int>(n + 1, 0));
        int ans = 0;
        for (int i = 0; i < 3 * n; i++) {
            for (int j = 1; j <= n; j++) {
                if (i >= 2) {
                    dp[i][j] = max(slices[i] + dp[i - 2][j - 1], dp[i - 1][j]);
                }
                else if (i == 1) {
                    dp[i][j] = max(slices[i], dp[i - 1][j]);
                }
                else {
                    dp[i][j] = slices[i];
                }
            }
        }
        ans = dp[3 * n - 2][n];
        dp = vector<vector<int>>(3 * n, vector<int>(n + 1, 0));
        for (int i = 1; i < 3 * n; i++) {
            for (int j = 1; j <= n; j++) {
                if (i >= 2) {
                    dp[i][j] = max(slices[i] + dp[i - 2][j - 1], dp[i - 1][j]);
                }
                else if (i == 1) {
                    dp[i][j] = max(slices[i], dp[i - 1][j]);
                }
                else {
                    dp[i][j] = slices[i];
                }
            }
        }
        ans = max(ans, dp[3 * n - 1][n]);
        return ans;
    }
};
```