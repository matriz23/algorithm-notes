---
title: LeetCode-1717 删除子字符串的最大得分 
date: 2022-09-03 16:26:00
updated:
tags: [计算机, 算法, LeetCode, C++, 字符串, 贪心]
categories: LeetCode题解
cover: https://raw.githubusercontent.com/wsylab/wsyPicGo/main/img/photo-1497089868400-ca22b72b719e
description: LeetCode-1717「删除子字符串的最大得分」的思考与解答
---
### [LeetCode-1717 删除子字符串的最大得分](https://leetcode.cn/problems/maximum-score-from-removing-substrings/)

### Solution 1
对字符串不断进行配对删除的操作可以通过栈模拟来快速实现. 本题中, 我们考虑 $ab$ 和 $ba$ 两种模式串, 优先匹配哪一种. 第一次匹配分高的, 第二次匹配分低的, 就能得到最高分.
代码如下:
```C++
class Solution {
public:
    int maximumGain(string s, int x, int y) {
        string high, low;
        int high_score, low_score;
        if (x >= y) {
            high_score = x;
            low_score = y;
            high = "ab";
            low = "ba";
        }
        else {
            high_score = y;
            low_score = x;
            high = "ba";
            low = "ab";
        }
        string res = "";
        int n = s.size();
        int ans = 0;
        for (int i = 0; i < n; i++) {
            if (res.size() != 0 && res[res.size() - 1] == high[0] && s[i] == high[1]) {
                ans += high_score;
                res.pop_back();
            }
            else {
                res.push_back(s[i]);
            }
        }
        s = res;
        res = "";
        for (int i = 0; i < s.size(); i++) {
            if (res.size() != 0 && res[res.size() - 1] == low[0] && s[i] == low[1]) {
                ans += low_score;
                res.pop_back();
            }
            else {
                res.push_back(s[i]);
            }
        }
        return ans;
    }
};
```