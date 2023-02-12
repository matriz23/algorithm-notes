---
title: CodeForces-1084C The Fair Nut and String 
date: 2022-07-14 10:24:00
updated:
tags: [计算机, 算法, CodeForces, C++, 字符串, 计数]
categories: CodeForces题解
cover: https://raw.githubusercontent.com/wsylab/wsyPicGo/main/img/photo-1626697556426-8a55a8af4999
description: CodeForces-1084C「The Fair Nut and String」的思考与解答
---
### [CodeForces-1084C The Fair Nut and String](https://codeforces.com/problemset/problem/1084/C)
### 题目大意
给出一个只包含小写字母的字符串 $s$ , 求满足以下条件的子序列(不要求连续)的个数：
- 这个子序列中的所有字符都是小写字母 $a$。
- 如果这个子序列的长度大于$1$, 原序列中在这个子序列中的每两个字母之间一定要包含一个小写字母 $b$ .

### Solution 1
显然答案只和原序列中的 $a$ 和 $b$ 有关, 并且和 $b$ 的位置关系密切. 对于不含 $b$ 且含有 $k$ 个 $a$ 的一段子字符串, 我们要么从中选一个, 要么不选, 共有 $k + 1$ 种选择, 用乘法原理相乘得到所有可能的选择. 但是全不选(即空字符串)不符合答案, 减去这一种情况就得到最终答案.
代码如下:
```C++
#include <bits/stdc++.h>
using namespace std;
const int MOD = 1e9 + 7;
int main() {
    string str;
    cin>>str;
    str += 'b';
    long long ans = 1;
    long long cnt = 0;
    for (int i = 0; i < str.size(); i++) {
        if (str[i] == 'a') {
            cnt++;
        }
        else if (str[i] == 'b') {
            ans *= cnt + 1;
            ans %= MOD;
            cnt = 0;
        }
    }
    cout<<ans - 1<<endl;
    return 0;
}
```
