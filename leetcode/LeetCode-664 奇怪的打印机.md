---
title: LeetCode-664 奇怪的打印机 
date: 2022-09-08 15:47:00
updated:
tags: [计算机, 算法, LeetCode, C++]
categories: LeetCode题解
cover: 
description: LeetCode-664「奇怪的打印机」的思考与解答
---
### [LeetCode-664 奇怪的打印机](https://leetcode.cn/problems/strange-printer/)

### Solution 1
记 $dp[i][j]$ 为打印 $s[i: j]$ 的最少次数. 首先打印首字母一定是最优的. 如果 $s[i] == s[j]$ , 那么打印 $s[i]$ 时可以顺便打印 $s[j]$ , 原问题与 $dp[i][j - 1]$ 一样. 否则, 我们需要把枚举分界点, 把这一段分成两段打印. 状态转移方程如下:
$$
dp[i][j] = 
\begin{cases}
dp[i][j - 1],\ s[i] = s[j]\\
\underset{i\leq k<j}{min}\{dp[i][k] + dp[i + 1][j]\},\ s[i] \not= s[j]\\
\end{cases}
$$
代码如下:
```C++
class Solution {
public:
    map<string, int> book;
    vector<vector<int>> dp;
    int n;
    string t = "";
    int f(int l, int r) {
        if (l == r) {
            return 1;
        }
        if (l > r) {
            return 0;
        } 
        if (dp[l][r] != 1e9) {
            return dp[l][r];
        }
        int res = 1e9;
        for (int i = l; i <= r - 1; i++) {
            res = min(res, f(l, i) + f(i + 1, r));
        }
        if (t[l] == t[r]) {
            res = min(res, f(l, r - 1) + 1);
        }
        dp[l][r] = res;
        cout<<l<<", "<<r<<": "<<t.substr(l, r - l + 1)<<"  "<<res<<endl;
        return res;
    }

    int strangePrinter(string s) {
        for (char ch: s) {
            if (t.size() == 0 || t[t.size() - 1] != ch) {
                t.push_back(ch);
            }
        }
        cout<<t<<endl;
        n = t.size();
        dp = vector<vector<int>> (n, vector<int>(n, 1e9));
        return f(0, n - 1);
    }
};
```