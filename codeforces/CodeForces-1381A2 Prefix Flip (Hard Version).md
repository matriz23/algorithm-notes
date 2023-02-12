---
title: CodeForces-1381A2 Prefix Flip (Hard Version) 
date: 2022-08-01 21:24:00
updated:
tags: [计算机, 算法, CodeForces, C++, 构造, 字符串]
categories: CodeForces题解
cover: https://raw.githubusercontent.com/wsylab/wsyPicGo/main/img/photo-1503642551022-c011aafb3c88.jpg
description: CodeForces-1381A2「Prefix Flip (Hard Version)」的思考与解答
---
### [CodeForces-1381A2 Prefix Flip (Hard Version)](http://codeforces.com/problemset/problem/1381/A2)
### 题目大意
给定两个长度为 $n$ 的 $01$ 字符串 $a, b$ , 每次操作可以选择 $a$ 的一个前缀, 先把 $0$ 变成 $1$ , $1$ 变成 $0$ , 再反转前缀. 要求用不超过 $2n$ 次操作把 $a$ 变成 $b$ , 输出操作序列.
### Solution 1
容易想到的操作就是模拟, 从后往前操作. 用至多 $2$ 次操作就可以把一位变成与 $b$ 相同. 这种方法可以通过本题的 Easy 版本, 但 $O(n^2)$ 的复杂度不足以通过 Hard 版本. 能否从一个角度思考, 不需要修改字符串本身呢? 一种巧妙的解法是, 我们把 $a$ 变成 $b$ 的过程拆分成两个子过程: $a$ 变为全 $0/1$ 字符串, 再由全 $0/1$ 字符串变为 $b$ . 进一步转化, 可以看作把 $a$ 和 $b$ 分别变为全 $0/1$ 字符串, 如果两个全 $0/1$ 字符串不同, 再用一次操作反转即可. 其中 $b$ 的操作是对 $a$ 的操作的逆序, 我们应该倒着输出. 把 $a, b$ 变成全 $0/1$ 字符串只需要检查每一位与后一位是否相同, 如果不同就对整个前缀进行操作即可, 对于每个字符串, 这一步至多需要 $n - 1$ 步, 总共的步骤至多为 $2\times (n - 1) + 1 = 2
n - 1$ 步, 符合要求.
代码如下:
```C++
#include <bits/stdc++.h>
using namespace std;
int main() {
    int t;
    cin>>t;
    while (t--) {
        int n;
        string a, b;
        cin>>n>>a>>b;
        vector<int> a_o, b_o;
        a_o.resize(0);
        b_o.resize(0);
        for (int i = 0; i < n - 1; i++) {
            if (a[i] != a[i + 1]) {
                a_o.push_back(i + 1);
            }
            if (b[i] != b[i + 1]) {
                b_o.push_back(i + 1);
            }
        }
        if (a[n - 1] != b[n - 1]) {
            a_o.push_back(n);
        }
        cout<<a_o.size() + b_o.size()<<" ";
        for (int x: a_o) {
            cout<<x<<" ";
        }
        for (int i = b_o.size() - 1; i >= 0; i--) {
            cout<<b_o[i]<<" ";
        }
        cout<<endl;
    }
    return 0;
}
```