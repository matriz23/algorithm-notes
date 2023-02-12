---
title: LeetCode-139 单词拆分
date: 2022-07-11 17:44:00
updated:
tags: [计算机, 算法, LeetCode, C++, 字符串, 动态规划]
categories: LeetCode题解
cover: https://raw.githubusercontent.com/wsylab/wsyPicGo/main/img/photo-1561651188-d207bbec4ec3
description: LeetCode-139「单词拆分」的思考与解答
---
### [LeetCode-139 单词拆分](https://leetcode.cn/problems/word-break/)

### Solution 1
我们考虑题中拼接的特殊性质: 如果字符串 $s_1$ 和 $s_2$ 都合法, 那么 $s_1 + s_2$ 也是合法的, 也就是拼接一个字符串可以分解为相同的子问题. 从这一点出发, 我们考虑把字符串进行分割处理., 对于一个长度为 $n$ 的字符串 $s$ , 分割为 $s[0: i - 1]$ 与 $s[i: n - 1]$ . 为了计算 $s[0: i - 1]$ 的可分性, 我们不妨用 $dp[i]$ 数组记录 $s$ 前 $i$ 位的可分性, 有初始情况 $dp[0] = ture$ (这是因为空字符串自然是可分的). 计算 $dp[i]$ 时, 利用之前所说的枚举分割点 $j$ , 有状态转移方程
$$
dp[i] = \sum_{0\leq j\leq i - 1} dp[j] \land isValid(s[j: i -1])
$$
怎么判断 $isValid(s[j: i -1])$ 呢? 是不是还要用同样的方法判别一遍呢? 其实不需要, 判断 $s[j: i -1]$ 是否在单词集合 $wordDict$ 中即可. 因为左侧部分的 $dp[j]$ 会枚举所有可能性的点, 因此右侧只需要枚举单独一个单词的情况即可. 想到这一点, 我们可以把 $j$ 的枚举范围进一步压缩, 记 $m = \underset{word\in wordDict}{max}word.size()$, 则 $i - j$ 不能大于 $m$ , 否则右侧不可能与现有单词匹配. 改进后的状态转移方程如下:
$$
dp[i] = \sum_{i - m\leq j\leq i - 1} dp[j]\land wordDict.count(s[j: i - 1])
$$
代码如下:
```C++
class Solution {
public:
    bool wordBreak(string s, vector<string>& wordDict) {
        set<string> book;
        int maxWordLen = 0;
        for (auto word: wordDict) {
            book.insert(word);
            maxWordLen = max(maxWordLen, (int)(word.size()));
        }
        int n = s.size();
        vector<bool> dp(n + 1, false);
        dp[0] = true;
        for (int i = 1; i <= n; i++) { // 判断0-(i-1)是否能被拼出
            for (int j =  i - 1; j >= i - maxWordLen && j >= 0; j--) { // 枚举分割点j进行状态转移
                dp[i] = dp[i] | (dp[j] & book.count(s.substr(j, i - j)));
            }
        }
        return dp[n];
    }
};
```