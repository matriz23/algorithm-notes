---
title: LeetCode-2488 统计中位数为 K 的子数组 
date: 2022-12-15 10:40:00
updated:
tags: [计算机, 算法, LeetCode, C++]
categories: LeetCode题解
cover: 
description: LeetCode-2488「统计中位数为 K 的子数组」的思考与解答
---
### [LeetCode-2488 统计中位数为 K 的子数组](https://leetcode.cn/problems/count-subarrays-with-median-k/)

### Solution 1
中位数只和大小关系有关. 只关心以 $k$ 为中位数的子数组个数, 因此把大于 $k$ 的元素记为 $1$, 小于 $k$ 的元素记为 $-1$ , 把 $k$ 记为 $0$ . 我们要找的子数组就是和为 $0$ 或 $1$ 且包含 $0$ 的子数组. 
通过前缀和来统计. 首先计算 $k$ 左侧位置的前缀和, 用哈希表 $book$ 来存储数量. 接下来计算 $k$ 及其右侧位置的前缀和. 对于 $k$ 及其右侧位置的前缀和 $s$ , 有 $book[s]$ 以及 $book[s - 1]$ 个子数组和为 $0$ 或 $1$ 且包含 $0$ . 
需要注意的是, $book$ 首先应当存储一个 $0$ , 可以看作左侧为空情况下的前缀和 (对应 $nums[0...m]$ 的情况) , 这样才不会遗漏答案.
代码如下:
```C++
class Solution {
public:
    int countSubarrays(vector<int>& nums, int k) {
        int idx = -1;
        for (int i = 0; i < nums.size(); i++) {
            if (nums[i] == k) {
                nums[i] = 0;
                idx = i;
            }
            else if (nums[i] < k) {
                nums[i] = -1;
            }
            else {
                nums[i] = 1;
            }
        }
        int ans = 0;
        map<int, int> book;
        int sum = 0;
        book[0] = 1; // 空子数组的前缀和
        for (int i = 0; i < idx; i++) {
            sum += nums[i];
            book[sum]++;
        }
        for (int i = idx; i < nums.size(); i++) {
            sum += nums[i];
            ans += book[sum] + book[sum - 1];
        }
        return ans;
    }
};
```