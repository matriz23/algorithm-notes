---
title: LeetCode-805 数组的均值分割 
date: 2022-11-14 11:44:00
updated:
tags: [计算机, 算法, LeetCode, C++]
categories: LeetCode题解
cover: 
description: LeetCode-805「数组的均值分割」的思考与解答
---
### [LeetCode-805 数组的均值分割](https://leetcode.cn/problems/split-array-with-same-average/)

### Solution 1
设数组 $nums$ 分割成了两个均值相等的子数组 $A, B$ , 则 $nums$ 的均值与 $A$ 的均值也相等. 我们可以把 $nums$ 中的每个元素先乘 $n$ 再减去 $sum$ , 那么均值相等意味着和为 $0$ . 现在我们的目标变成了从 $nums$ 中选出一个和为 $0$ 的真子集 $A$ .
如果直接从整个 $nums$ 中选 $A$ , 枚举范围是 $2^{n}$ , 会超时; 考虑把 $nums$ 先拆分成左右两半 $S_1, S_2$ , 如果 $S_1$ 有某个子集和为 $0$ , 那么返回 `true` ; 对 $S_2$ 同理. 第三种情况是 $S_1$ 的子集和 $S_2$ 的子集的并集和为 $0$ , 我们可以用哈希表存储 $S_1$ 的枚举结果, 在 $S_2$ 枚举时进行比对. 
需要注意 $S_1$ 和 $S_2$ 全选是不符合题意的, 这一点在实现时可以用不全选 $S_2$ 代替. 这样会不会导致漏掉 $S_2 \cup$ $(S_1$ 的某个真子集 $)$ 呢? 事实是不会的. 假设存在这种情况, 那么其补集和也为 $0$ , 在枚举 $S_1$ 的子集时已经考虑过了.
代码如下:
```C++
class Solution {
public:
    bool splitArraySameAverage(vector<int>& nums) {
        int n = nums.size();
        if (n == 1) {
            return false;
        }
        int s = accumulate(nums.begin(), nums.end(), 0);
        for (int &num: nums) {
            num = num * n - s;
        }
        int m = n >> 1;
        set<int> book;
        for (int i = 1; i < 1 << m; i++) {
            int t = 0;
            for (int j = 0; j < m; j++) {
                if (i & 1 << j) {
                    t += nums[j];
                }
            } 
            if (t == 0) {
                return true;
            }
            book.insert(t);
        }
        for (int i = 1; i < 1 << (n - m); i++) {
            int t = 0;
            for (int j = 0; j < n - m; j++) {
                if (i & 1 << j) {
                    
                    t += nums[m + j];
                }
            }
            if (t == 0 || (book.count(-t) && i != (1 << (n - m)) - 1)) {
                return true;
            }
        }
        return false;
    }
};
```