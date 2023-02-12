---
title: LeetCode-2386 找出数组的第 K 大和 
date: 2022-08-23 16:27:00
updated:
tags: [计算机, 算法, LeetCode, C++]
categories: LeetCode题解
cover: https://raw.githubusercontent.com/wsylab/wsyPicGo/main/img/photo-1530738433046-b589e02b0e79
description: LeetCode-2386「找出数组的第 K 大和」的思考与解答
---
### [LeetCode-2386 找出数组的第 K 大和](https://leetcode.cn/problems/find-the-k-sum-of-an-array/)

### Solution 1
先考虑一个简化版的问题: 给定 $n$ 个非负数 $a_0, a_1, ..., a_n$ , 如何找出第 $k$ 小的子序列和? 
不妨设 $a$ 是从小到大排好序的. 最小的子序列和显然是 $0$ . 记 $(s, i)$ 为总和为 $s$ , 且最后一个元素下标为 $i$ 的子序列. 我们用一个最小堆来维护这些子序列. 一开始堆中只有 $(a_0, 0)$ , 取出堆顶元素时, 可以进行以下操作:
- 把 $a_{i + 1}$ 接到这个子序列后面, 把 $(s + a_{i + 1}, i + 1)$ 放到堆中;
- 把 $a_i$ 替换成 $a_{i + 1}$ , 把 $(s - a_i + a_{i + 1}, i + 1)$ 放到堆中.

可以看出, 这种策略保证了:
- 所有的子序列都会被遍历到;
- 子序列的遍历顺序由小到大.

通过这种操作, 我们可以很快得到第 $k$ 小的子序列.
对于本题, 要求寻找第 $k$ 大的子序列, 而且数组元素包含负数, 需要寻找一种合适的转化. 首先可以得到最大的子序列和, 把 $a$ 中的非负元素相加就可以得到. 怎么修改这个子序列呢? 对于一个非负元素, 我们可以从子序列和中把它减去来得到更小的子序列; 对于一个负元素, 我们可以在子序列之外加上这个元素来得到更小的子序列. 这两种操作可以合并成减去原数组中某些元素的绝对值. 要得到第 $k$ 大的子序列, 也就是要找到 $\vert a\vert$ 中第 $k$ 小的子序列. 这样, 我们就把本题转化成了简化版的问题.
代码如下:
```C++
#define ll long long
typedef pair<ll, int> pii;
class Solution {
public:
    long long kSum(vector<int>& nums, int k) {
        ll sum = 0;
        for (int &num: nums) {
            if (num > 0) {
                sum += num;
            }
            else {
                num = -num;
            }
        }
        sort(nums.begin(), nums.end());
        priority_queue<pii> pq; // 我们直接用最大堆来合并两个步骤
        pq.emplace(sum, 0); // 当前和为 sum , 减去的子序列最大下标为 i - 1
        while (--k) { // 循环 k - 1 次
            auto[sum, i] = pq.top();
            pq.pop();
            if (i < nums.size()) {
                pq.emplace(sum - nums[i], i + 1); // 减去的子序列再减去 nums[i]
                if (i != 0) {
                    pq.emplace(sum - nums[i] + nums[i - 1], i + 1); // 减去的子序列补上前面的nums[i - 1], 再减去 nums[i]
                }
            }
        }
        return pq.top().first;
    }
};
```