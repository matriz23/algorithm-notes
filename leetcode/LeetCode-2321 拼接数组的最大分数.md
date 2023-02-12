---
title: LeetCode-2321 拼接数组的最大分数 
date: 2022-06-28 14:29:00
updated:
tags: [计算机, 算法, LeetCode, C++, 动态规划, 前缀和]
categories: LeetCode题解
cover: https://raw.githubusercontent.com/wsylab/wsyPicGo/main/img/photo-1656231928735-c6651c2c4ae3
description: LeetCode-2321「拼接数组的最大分数」的思考与解答
---
### [LeetCode-2321 拼接数组的最大分数](https://leetcode.cn/problems/maximum-score-of-spliced-array/)

### Solution 1
如果我们选择交换 $[left, right]$ 区间, 则对于 $nums1$ 而言, 新的数组和为 
$$
\sum_{i = 0}^{left - 1}nums1[i] + \sum_{i=left}^{right}nums2[i] + \sum_{i=right + 1}^{n - 1}nums1[i]
$$
变形为:
$$
\sum_{i = 0}^{n - 1}nums[1] + \sum_{i = left}^{right}(nums2[i] - nums1[i])
$$
记 $diff[i] = nums2[i] - nums1[i]$ , 我们求出 $diff$ 的连续子数组最大和即可. 这一部分的处理是很经典的动态规划, 在此不再赘述.
对于 $nums2$ 而言, 处理方法类似, 交换后新的数组和为
$$
\sum_{i = 0}^{n - 1}nums2[i] - \sum_{i = left}^{right}(nums2[i] - nums1[i])
$$
这里应当求 $diff$ 的连续子数组最小和.
最后不要忘记考虑不交换的情形, 把 $ans$ 与 $nums1, nums2$ 的数组和比较一下.
代码如下:
```C++
class Solution {
public:
    int maximumsSplicedArray(vector<int>& nums1, vector<int>& nums2) {
        int n = nums1.size();
        vector<int> diff(n, 0);
        int sum1 = 0, sum2 = 0;
        for (int i = 0; i < n; i++) {
            diff[i] = nums2[i] - nums1[i];
            sum1 += nums1[i];
            sum2 += nums2[i];
        }
        int ans = max(sum1, sum2);
        vector<int> dp1(n, 0), dp2(n, 0);
        for (int i = 0; i < n; i++) {
            dp1[i] = (i == 0)? diff[i]: max(dp1[i - 1] + diff[i], diff[i]);
            dp2[i] = (i == 0)? diff[i]: min(dp2[i - 1] + diff[i], diff[i]);
        }
        for (int i = 0; i < n; i++) {
            ans = max(ans, max(sum1 + dp1[i], sum2 - dp2[i]));
        }
        return ans;
    }
};
```

