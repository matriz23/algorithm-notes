---
title: LeetCode-1703 得到连续 K 个 1 的最少相邻交换次数 
date: 2022-12-18 10:49:00
updated:
tags: [计算机, 算法, LeetCode, C++, 贪心]
categories: LeetCode题解
cover: 
description: LeetCode-1703「得到连续 K 个 1 的最少相邻交换次数」的思考与解答
---
### [LeetCode-1703 得到连续 K 个 1 的最少相邻交换次数](https://leetcode.cn/problems/minimum-adjacent-swaps-for-k-consecutive-ones/)

### Solution 1
记 $x_i$ 为数组中第 $i$ 个 $1$ 的下标. 假设最终得到的连续 $k$ 个 $1$ 最左侧的 $1$ 位置为 $m$ , 来自原数组中下标为 $x_t$ 的那个 $1$ . 显然, 下标为 $x_t, x_{t+1}, ..., x_{t+k - 1}$ 的 $1$ 构成了最终的连续 $k$ 个 $1$ , 它们一共移动了 $\sum_{i = 0}^{k - 1}\vert x_{t+i} - (m+i)\vert$ 距离, 也就是进行了同样多次数的相邻交换. 
问题转化成求解下列优化问题:
$$
\underset{t,m}{min}\ z = \sum_{i = 0}^{k - 1}\vert x_{t+i} - (m+i)\vert
$$

记 $y_i = x_i - i, m'=m-t$ , 则 $x_{t+i} - (m+i) = y_{t+i} - (m - t) = y_{t+i}-m'$ , 优化问题变为:
$$
\underset{t,m'}{min}\ z = \sum_{i = 0}^{k - 1}\vert y_{t+i}-m'\vert
$$
在给定 $t$ 的情况下, 易知 $m' = y_{t+\lfloor \frac{k}{2}\rfloor}$ 时最优, 此时目标值为 
$$z^*(t)= \underset{m'}{min}\ z\\=\sum_{i = 0}^{\lfloor \frac{k}{2}\rfloor - 1}(y_{t+\lfloor \frac{k}{2}\rfloor} - y_{t+i}) + \sum_{i = \lfloor \frac{k}{2}\rfloor}^{k - 1}(y_{t+i} - y_{t+\lfloor \frac{k}{2}\rfloor})\\=(2\times \lfloor \frac{k}{2}\rfloor - k) \times y_{t+\lfloor \frac{k}{2}\rfloor} -\sum_{i = 0}^{\lfloor \frac{k}{2}\rfloor - 1}y_{t+i} + \sum_{i = \lfloor \frac{k}{2}\rfloor}^{k - 1}y_{t+i} 
$$

通过预处理 $y_i$ 的前缀和, 我们可以在 $O(1)$ 时间内计算出 $z^*(t)$ , 从而可以在 $O(n)$ 时间内解决问题.
代码如下:
```C++
#define ll long long
class Solution {
public:
    int minMoves(vector<int>& nums, int k) {
        int cnt = accumulate(nums.begin(), nums.end(), 0);
        int x[cnt];
        int pre[cnt];
        for (int i = 0, j = 0; i < nums.size() && j < cnt; i++) {
            if (nums[i] == 1) {
                x[j] = i - j;
                pre[j] = x[j] + (j == 0 ? 0 : pre[j - 1]);
                j++;
            }
        }
        ll ans = 1e12;
        for (int t = 0; t <= cnt - k; t++) {
            ll m = x[t + k / 2];
            ll res = (k / 2) * m -
                            (t + k / 2 - 1 < 0 ? 0 : pre[t + k / 2 - 1]) +
                            (t == 0 ? 0 : pre[t - 1]) + pre[t + k - 1] -
                            (t + k / 2 - 1 < 0 ? 0 : pre[t + (k / 2) - 1]) -
                            (k - k / 2) * m;
            ans = min(ans, res);
        }
        return ans;
    }
};
```