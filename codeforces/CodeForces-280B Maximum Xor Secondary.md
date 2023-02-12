---
title: CodeForces-280B Maximum Xor Secondary 
date: 2022-08-05 13:14:00
updated:
tags: [计算机, 算法, CodeForces, C++, 单调栈]
categories: CodeForces题解
cover: https://raw.githubusercontent.com/wsylab/wsyPicGo/main/img/sky-rose.jpg
description: CodeForces-280B「Maximum Xor Secondary」的思考与解答
---
### [CodeForces-280B Maximum Xor Secondary](http://codeforces.com/problemset/problem/280/B)
### 题目大意
给定一个长度为 $n$ 的数组 $a$ , 求 $a$ 的所有长度 $\geq 2$ 的子数组中最大值与次大值的异或值的最大值.
### Solution 1
异或不是关键, 关键是怎样快速找出可能的最大值与次大值对. 一种相对常规的想法是, 我们固定最大值或次大值为 $a_i$ , 向左右寻找合法的次大值. 固定次大值寻找第一个 $\geq a_i$ 的元素更容易一些, 这部分可以使用单调栈处理. 遍历元素时, 不断弹出栈顶元素直到栈空或者栈顶元素 $\geq$ 当前元素, 如果栈空, 说明当前元素在这一侧为最大值, 不存在合法数对; 否则, 我们就找到了以当前元素为次大值, 其中一侧的最近的最大值, 这是唯二合法的数对之一. 更新 $ans$ 之后, 把当前元素入栈即可. 用正反两次遍历可以找到所有最大值与次大值的匹配.
代码如下:
```C++
#include <bits/stdc++.h>
using namespace std;
#define ll long long
const ll MOD = 1e9 + 7;
int main() {
    int n;
    cin>>n;
    vector<int> a(n, 0);
    for (int i = 0; i < n; i++) {
        cin>>a[i];
    }
    stack<int> st;
    int ans = 0;
    for (int i = 0; i < n; i++) {
        while (!st.empty() && st.top() < a[i]) {
            st.pop();
        }
        if (!st.empty()) {
            ans = max(a[i] ^ st.top(), ans);
        }
        st.push(a[i]);
    }
    for (int i = n - 1; i >= 0; i--) {
        while (!st.empty() && st.top() < a[i]) {
            st.pop();
        }
        if (!st.empty()) {
            ans = max(a[i] ^ st.top(), ans);
        }
        st.push(a[i]);
    }
    cout<<ans<<endl;
    return 0;
}
```