---
title: CodeForces-1217C The Number Of Good Substrings 
date: 2022-08-24 15:50:00
updated:
tags: [计算机, 算法, CodeForces, C++, 字符串, 贪心]
categories: CodeForces题解
cover: https://raw.githubusercontent.com/wsylab/wsyPicGo/main/img/photo-1485765264175-4cc94fa15c57
description: CodeForces-1217C「The Number Of Good Substrings」的思考与解答
---
### [CodeForces-1217C The Number Of Good Substrings](https://codeforces.com/problemset/problem/1217/C)
### 题目大意
给定 $01$ 字符串 $s$ , 求 $s$ 有多少个子串 $t$ 满足 $t$ 转化成十进制后的值与长度相等.

### Solution 1
先不考虑前导 $0$ . 由于子串 $t$ 的值是指数级增长的, 而 $t$ 的长度却是线性增长的. 这意味着在 $t$ 前面一般都需要一些前导 $0$ 来补足差距. 注意到前导 $0$ 的数量减少也不会影响 $t$ 的值, 所以我们贪心地寻找连续的一段 $0$ 的长度以最大化先导 $0$ 的个数, 记为 $cnt$ (这里的 $cnt$ 可以为 $0$ , 子串 $1$ 前面不需要前导 $0$ .) 从连续的这段 $0$ 开始, 逐步延长后继子串的长度 $len$ 的同时维护后继子串值 $res$ . 如果 $cnt + len >= res$ , 那么一定提供了一个合法的子串.
代码如下:
```C++
#include <bits/stdc++.h>
using namespace std;
#define ll long long
const ll MOD = 1e9 + 7;
int main() {
    int t;
    cin>>t;
    while (t--) {
        string s;
        cin>>s;
        int n = s.size();
        int ans = 0;
        int cnt = 0;
        for (int i = 0; i < n; i++) {
            if (s[i] == '0') {
                cnt++;
            }
            else {
                int res = 0;
                for (int j = 0; (1 << j) <= cnt + j + 1 && i + j < n; j++) {
                    res = res * 2 + (s[i + j] - '0');
                    if (res <= cnt + j + 1) {
                        ans++;
                    }
                }
                cnt = 0;
            }
        }
        cout<<ans<<endl;
    }
    return 0;
}
```