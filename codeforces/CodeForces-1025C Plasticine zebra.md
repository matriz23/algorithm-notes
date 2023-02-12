---
title: CodeForces-1025C Plasticine zebra 
date: 2022-07-14 15:19:00
updated:
tags: [计算机, 算法, CodeForces, C++, 构造, 思维, 字符串]
categories: CodeForces题解
cover: https://raw.githubusercontent.com/wsylab/wsyPicGo/main/img/photo-1578326457399-3b34dbbf23b8
description: CodeForces-1025C「Plasticine zebra」的思考与解答
---
### [CodeForces-1025C Plasticine zebra](https://codeforces.com/problemset/problem/1025/C)
### 题目大意
有一个由 $b$ 和 $w$ 构成的字符串 $s$ , 每次操作可以把 $s$ 分为两部分分别翻转, 求任意次操作能得到的最长的 $b$ , $w$ 交错的子字符串的长度.

### Solution 1
不要被翻转这一操作给迷惑了!
把 $s$ 分成 $s_1$ 和 $s_2$ 两部分, 颠倒后重置, 相当于把 $s_2$ 接到了 $s_1$ 的前面, 再颠倒. 把原字符串看成一个环, 我们要求的就是环上最长 $b$ , $w$ 交错子字符串的长度. 实际操作时可以把 $s$ 复制一份模拟环.
代码如下:
```C++
#include <bits/stdc++.h>
using namespace std;
int main() {
    string s;
    cin>>s;
    s = s + s;
    int cnt = 1;
    int ans = 1;
    for (int i = 1; i < s.size(); i++) {
        if (s[i] == s[i - 1]) {
            ans = max(ans, cnt);
            cnt = 1;
        }
        else {
            cnt++;
        }
    }
    ans = max(cnt, ans);
    cout<<min(ans, int(s.size()) / 2)<<endl; // 不能超过原字符串的长度
    return 0;
}
```