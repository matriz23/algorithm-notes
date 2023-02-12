---
title: CodeForces-1503A Balance the Bits 
date: 2022-07-29 15:18:00
updated:
tags: [计算机, 算法, CodeForces, C++, 字符串, 构造, 贪心]
categories: CodeForces题解
cover: https://raw.githubusercontent.com/wsylab/wsyPicGo/main/img/photo-1511097646266-61a6d20ed830.jpg
description: CodeForces-1503A「Balance the Bits」的思考与解答
---
### [CodeForces-1503A Balance the Bits](https://codeforces.com/problemset/problem/1503/A)
### 题目大意
给定一个长度为 $n$ 的 $01$ 字符串 $s$ , 根据 $s$ 构造两个长度同样为 $n$ 的括号字符串 $a, b$ , 满足:
- $a, b$ 都是合法的括号匹配字符串;
- $a[i]$ 与 $b[i]$ 相同当且仅当 $a[i]$ 为 $1$ .

如果存在合法构造, 输出 $YES$ 并给出一个构造; 否则输出 $NO$ .
### Solution 1
假设存在合法构造, 设 $a, b$ 相同的部分中, $($ 有 $x$ 个, $)$ 有 $y$ 个; 不同的部分中, 考虑 $a$ , $($ 有 $z$ 个, $)$ 有 $w$ 个, 则由 $a$ 的括号匹配, 有 $x + z = y + w$ ; 再由 $b$ 的括号匹配, 有 $x + w = y + z$ , 得到 $x = y, z = w$ , 故原字符串至少满足 $0$ 和 $1$ 的个数都是偶数. 得到这一限制后, 我们来尝试构造 $a, b$ . 由于括号的数量匹配已经有保证了, 下面尽可能满足 $($ 数量不少于 $)$ 数量即可. 对于 $1$ 的前一半, 我们全部选择 $($ , 对于后一半全部选择 $)$ . 对于 $0$ , 由于 $a, b$ 的对称性, 我们交替选择 $($ 与 $)$ . 这种贪心的构造方法能够保证如果存在解, 这种方法一定能构造出合法的. 在构造过程中, 检验到 $)$ 数量大于 $($ 则说明不存在合法构造.
代码如下:
```C++
#include "bits/stdc++.h"
using namespace std;
int main() {
    int t;
    cin>>t;
    while (t--) {
        int n;
        cin>>n;
        string s;
        cin>>s;
        if (n % 2 != 0) {
            cout<<"NO"<<endl;
            continue;
        }
        int num_0 = 0, num_1 = 0;
        for (auto ch: s) {
            num_0 += ch == '0';
            num_1 += ch == '1';
        }
        if (num_0 % 2 != 0) {
            cout<<"NO"<<endl;
            continue;
        }
        int cnt_0 = 0, cnt_1 = 0;
        int cnt = 0;
        string a = "", b = "";
        int flag = 0;
        for (auto ch: s) {
            if (ch == '1') {
                cnt++;
                if (cnt <= num_1 / 2) {
                    a += '(';
                    cnt_0++;
                    b += '(';
                    cnt_1++;
                }
                else {
                    a += ')';
                    cnt_0--;
                    b += ')';
                    cnt_1--;
                }

            }
            else {
                if (flag == 0) {
                    a += '(';
                    cnt_0++;
                    b += ')';
                    cnt_1--;
                }
                else {
                    a += ')';
                    cnt_0--;
                    b += '(';
                    cnt_1++;
                }
                flag = 1 - flag;
            }
            if (cnt_0 < 0 || cnt_1 < 0) {
                break;
            }
        }
        if (cnt_0 < 0 || cnt_1 < 0) {
            cout<<"NO"<<endl;
            continue;
        }
        cout<<"YES"<<endl;
        cout<<a<<endl;
        cout<<b<<endl;
    }
    return 0;
}
```