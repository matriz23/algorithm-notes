---
title: LeetCode-76 最小覆盖子串 
date: 2022-05-05 11:40:00
updated:
tags: [计算机, 算法, LeetCode, C++, 字符串, 滑动窗口]
categories: LeetCode题解
cover: https://raw.githubusercontent.com/wsylab/wsyPicGo/main/img/photo-1471733091092-73a03861dea7
description: LeetCode-76「最小覆盖子串」的思考与解答
---
### [LeetCode-76 最小覆盖子串](https://leetcode.cn/problems/minimum-window-substring/)

> 参考文章: [我写了首诗，把滑动窗口算法算法变成了默写题 ](https://labuladong.github.io/algo/2/18/25/)
### Solution 1
本题可以说是滑动窗口的代表了. 
根据需求, 我们需要寻找包含目标字符串中所有字符的一个连续字串, 考虑利用哈希表存储目标与当前子串. 一个需要注意的点是对于当前子串也只需要考虑目标中含有的字符.
对于滑动窗口, 一共分为四步, 扩大窗口, 更新数据&判断,, 缩小窗口, 更新数据&判断, 本质上是一个**寻找可行解、优化可行解**的过程.
代码如下:
```C++
class Solution {
public:
    string minWindow(string s, string t) {
        unordered_map<char, int> need, window;
        for (char c: t) {
            need[c]++;
        }
        // 左闭右开,[left, right)
        int right = 0;
        int left = 0;
        // valid记录当前窗口与目标的匹配的字符种类数
        int valid = 0;
        // 记录答案的左右端点
        int start = 0, len = INT_MAX;
        // 开始滑动
        while (right < s.size()) {
            // 扩大窗口
            char c = s[right];
            right++;
            // 更新数据
            if (need.count(c)) {
                window[c]++;
                if (window[c] == need[c]) {
                    valid++;
                }
            }
            // 判断, 如果是可行解, 那么优化
            while(valid == need.size()) {
                if (right - left < len) {
                    start = left;
                    len = right - left;
                }
                // 缩小窗口
                char d = s[left];
                left++;
                // 更新数据, 注意这一部分与扩大窗口的更新对称
                if (need.count(d)) {
                    if (window[d] == need[d]) {
                        valid--;
                    }
                    window[d]--;
                }
            } 
        }
        // 返回结果
        return len == INT_MAX ? "" : s.substr(start, len);
    }
};
```