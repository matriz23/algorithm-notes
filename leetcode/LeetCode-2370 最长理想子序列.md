---
title: LeetCode-2370 最长理想子序列 
date: 2022-08-18 22:45:00
updated:
tags: [计算机, 算法, LeetCode, C++, 字符串, 动态规划]
categories: LeetCode题解
cover: https://raw.githubusercontent.com/wsylab/wsyPicGo/main/img/photo-1538407798318-5c4ed97e8706.jpg
description: LeetCode-2370「最长理想子序列」的思考与解答
---
### [LeetCode-2370 最长理想子序列](https://leetcode.cn/problems/longest-ideal-subsequence/)

### Solution 1
对于子序列问题, 首先想到动态规划. 状态的转移和末尾元素有关, 因此我们用 $dp[ch]$ 记录当前以 $ch$ 字符结尾的最长理想子序列. 从前向后遍历 $s$ 时, 更新 $dp$ 数组即可.
代码如下:
```C++
class Solution {
public:
    int longestIdealString(string s, int k) {
        vector<int> dp(26, 0);
        dp[s[0] - 'a'] = 1;
        for (int i = 1; i < s.size(); i++) {
            int temp = 0;
            for (int ch = 0; ch < 26; ch++) {
                if (abs(s[i] - 'a' - ch) <= k) {
                    temp = max(temp, dp[ch] + 1);
                }
            }
            dp[s[i] - 'a'] = temp;
        }
        int ans = 0;
        for (auto res: dp) {
            cout<<res<<" ";
            ans = max(ans, res);
        }
        cout<<endl;
        return ans;
    }
};
```