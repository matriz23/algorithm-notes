---
title: LeetCode-2364 统计坏数对的数目 
date: 2022-08-18 12:28:00
updated:
tags: [计算机, 算法, LeetCode, C++, 哈希]
categories: LeetCode题解
cover: https://raw.githubusercontent.com/wsylab/wsyPicGo/main/img/photo-1504121449080-76ab9eb5bd2b
description: LeetCode-2364「统计坏数对的数目」的思考与解答
---
### [LeetCode-2364 统计坏数对的数目](https://leetcode.cn/problems/count-number-of-bad-pairs/)

### Solution 1
求 $j - i \not = nums[j] - nums[i]$ 的 "坏数对" 个数, 不妨改为求 $j - i = nums[j] - nums[i]$ 的 "好数对" 个数, 式子变形为 $j - nums[j] = i - nums[i]$ , 这样把二元性质转化成了一元的性质. 统计好数对个数时只需要考虑 $nums[i]$ 自身的值, 使用 $map$ 存储不同 $i - nums[j]$ 值的个数, 统计出 "好数对" 个数后用总数减去这个值就得到所求答案.
代码如下:
```C++
class Solution {
public:
    long long countBadPairs(vector<int>& nums) {
        long long n = nums.size();
        long long ans = n * (n - 1) / 2;
        unordered_map<int, long long> book;
        for (int i = 0; i < n; i++) {
            book[i - nums[i]]++;
        }
        for (auto it = book.begin(); it != book.end(); it++) {
            ans -= it->second * (it->second - 1) / 2;
        }
        return ans;
    }
};
```