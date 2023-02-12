---
title: CodeForces-1203D2 Remove the Substring (hard version) 
date: 2022-07-25 10:47:00
updated:
tags: [计算机, 算法, CodeForces, C++, 动态规划, 字符串]
categories: CodeForces题解
cover: https://raw.githubusercontent.com/wsylab/wsyPicGo/main/img/photo-1504208434309-cb69f4fe52b0
description: CodeForces-1203D2「Remove the Substring (hard version)」的思考与解答
---
### [CodeForces-1203D2 Remove the Substring (hard version)](https://codeforces.com/problemset/problem/1203/D2)
### 题目大意
给定两个字符串 $s$ 和 $t$ , 保证 $t$ 是 $s$ 的子序列(不要求连续) . 从 $s$ 中删除一段连续子串, 使得 $t$ 仍是 $s$ 的子序列,  求删除子串的可能最大长度.
### Solution 1
从 $s$ 中删除了一段连续子序列, 将 $s$ 断成了两截, 分别匹配 $t$ 的一部分. 从这一角度考虑, 计算出 $t$ 的前/后缀与 $s$ 的前/后缀的匹配情况. 记 $s[0:pre[i]]$ 与 $t[0:i]$ 匹配, $s[suf[i]: n - 1]$ 与 $t[i: m - 1]$ 匹配. 我们要计算删除子串的最大长度, 就希望前缀的匹配尽可能靠前, 后缀的匹配尽可能靠后, 遍历 $t$ 的分界点 $i$ , 对于给定的 $i$ , 最大子串的长度为 $suf[i + 1] - pre[i] - 1$ .
代码如下:
```C++
#include "bits/stdc++.h"
using namespace std;
int main() {
    string s, t;
    cin>>s>>t;
    int n = s.size(), m = t.size();
    // pre[i]: t[0 - i] 字符至少需要 s[0 - pre[i]] 来匹配
    // suf[i]: t[i - (m - 1)] 字符至少需要 s[suf[i] - n] 来匹配
    // 枚举计算 ans 即可, t 的 0 - i 被 0 - pre[i] 覆盖, (i + 1) - m - 1被 suf[i + 1] - n 覆盖
    // 考虑动态规划计算 pre 和 suf
    vector<int> pre(m, 0), suf(m, 0);
    int p = 0;
    for (int i = 0; i < n; i++) {
        if (s[i] == t[p]) {
            pre[p] = i;
            p++;
            if (p == m) {
                break;
            }
        }
    }
    p = m - 1;
    for (int i = n - 1; i >= 0; i--) {
        if (s[i] == t[p]) {
            suf[p] = i;
            p--;
            if (p == -1) {
                break;
            }
        }
    }
    int ans = max(n - 1 - pre[m - 1], suf[0]); // 越界特殊处理
    for (int i = 0; i < m - 1; i++) {
        ans = max(ans, suf[i + 1] - pre[i] - 1);
    }
    cout<<ans<<endl;
    return 0;
}
```