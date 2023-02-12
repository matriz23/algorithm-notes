---
title: CodeForces-1348C Phoenix and Distribution 
date: 2022-07-02 16:33:00
updated:
tags: [计算机, 算法, CodeForces, C++, 构造, 字符串, 字典序]
categories: CodeForces题解
cover: https://raw.githubusercontent.com/wsylab/wsyPicGo/main/img/photo-1439209306665-700c9bca794c
description: CodeForces-1348C「Phoenix and Distribution」的思考与解答
---
### [CodeForces-1348C Phoenix and Distribution](https://codeforces.com/problemset/problem/1348/C)
### 题目大意
给定长度为 $n$ 的字符串 $s$ , 将其不重不漏重组成 $k$ 个字符串, 输出使得字典序最大的字符串字典序最小的分法所得到的字典序最大的字符串. 
### Solution 1
本题是一道很有趣的构造性问题. 要使得字典序最大的字符串最小, 首先我们考虑开头的第一个字符. 如果 $s$ 中字典序最小的字符 $c$ 个数 $>= k$ , 则每个开头都分配一个 $c$ 即可. 在考虑后面的字符, 如果全都一样, 我们尽可能均摊即可; 如果存在不一样的, 字典序最大的会决定字符串的字典序, 我们希望它尽可能靠后, 因此把所有的字符都"挤"到它前面; 如果 $s$ 中字典序最小的字符 $c$ 个数 $< k$ , 字典序最大的字符同样决定了字符串的字典序, 我们让其单独成为一个字符串.
通俗点讲就是: 已经彻底烂掉的, 我们就不管了; 不那么烂的, 我们抢救一下. 仔细思考这种策略是最优的.
代码如下:
```C++
#include <bits/stdc++.h>
using namespace std;
int main() {
    int t;
    scanf("%d", &t);
    while (t--) {
        int n, k;
        string s;
        cin>>n>>k>>s;
        sort(s.begin(), s.end());
        if (s[0] != s[k-1]) {
			cout<<s[k - 1]<<endl;
		}
        else if (s[k] != s[n-1]) {
            cout<<s[0];
            cout<<s.substr(k)<<endl;
        }
        else if (s[k] == s[n-1]) {
            cout<<s[0];
			for (int i = 1; i <= (n - k) / k; i++)
				cout<<s[n-1];
			if ((n - k) % k != 0) {
                cout<<s[n-1];
            }
            cout<<endl;
        }
    }
    return 0;
}
```