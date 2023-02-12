---
title: LeetCode-438 找到字符串中所有字母异位词 
date: 2022-06-23 22:54:00
updated:
tags: [计算机, 算法, LeetCode, C++, 滑动窗口]
categories: LeetCode题解
cover: https://raw.githubusercontent.com/wsylab/wsyPicGo/main/img/photo-1512484186986-88ff4e9313cc
description: LeetCode-438「找到字符串中所有字母异位词」的思考与解答
---
### [LeetCode-438 找到字符串中所有字母异位词](https://leetcode.cn/problems/find-all-anagrams-in-a-string/)

### Solution 1
本题是一道经典的滑动窗口问题, 同时有一个值得关注的处理思想: 如何判别**异位词**? 答案是利用哈希表存储元素构成, 对比是否为异位词.
这道题我们利用滑动窗口, 依照扩大窗口-更新数据-缩小窗口-更新数据的基本流程编写代码即可.
代码如下:
```C++
class Solution {
public:
    vector<int> findAnagrams(string s, string p) {
        vector<int> ans;
        // 创建两个哈希表, need存储目标字符串的元素构成, window存储当前窗口字符串的元素构成
        unordered_map<char, int> need, window;
        for (char c: p) {
            need[c]++;
        }
        int left = 0, right = 0; // [left, right) 左开右闭区间
        int valid = 0;
        while (right < s.size()) {
            char c = s[right];
            right++; // 扩大窗口
            if (need.count(c)) {
                window[c]++;
                if (window[c] == need[c]) {
                    valid++; // 数据更新
                }
            }
            while (right - left > p.size() && valid == need.size()) {
                char d = s[left];
                left++; // 缩小窗口
                if (need.count(d)) {
                    if (window[d] == need[d]) {
                        valid--; // 数据更新
                    }
                    window[d]--;
                }
            }
            // 判断一个合法的窗口是否是我们需要的
            if (valid == need.size()) {
                ans.push_back(left);
            }
        }
        return ans;
    }
};
```