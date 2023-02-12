---
title: LeetCode-828 统计子串中的唯一字符 
date: 2022-09-06 11:11:00
updated:
tags: [计算机, 算法, LeetCode, C++, 字符串]
categories: LeetCode题解
cover: https://raw.githubusercontent.com/wsylab/wsyPicGo/main/img/photo-1431512284068-4c4002298068.jpg
description: LeetCode-828「统计子串中的唯一字符」的思考与解答
---
### [LeetCode-828 统计子串中的唯一字符](https://leetcode.cn/problems/count-unique-characters-of-all-substrings-of-a-given-string/)

### Solution 1
采用贡献法, 考虑每一个字符在多少个子字符串只出现了一次. 统计每种字符出现的下标, 应用乘法原理即可.
代码如下:
```C++
class Solution {
public:
    int uniqueLetterString(string s) {
        int n = s.size();
        int ans = 0;
        vector<vector<int>> cnt(26, vector<int>(1, -1));
        for (int i = 0; i < n; i++) {
            cnt[s[i] - 'A'].push_back(i);
        }
        for (int i = 0; i < 26; i++) {
            cnt[i].push_back(n);
            for (int j = 1; j < cnt[i].size() - 1; j++) {
                ans += (cnt[i][j] - cnt[i][j - 1]) * (cnt[i][j + 1] - cnt[i][j]);
            }
        }
        return ans;
    }
};
```