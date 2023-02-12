---
title: CodeForces-1373D Maximum Sum on Even Positions 
date: 2022-07-20 12:57:00
updated:
tags: [计算机, 算法, CodeForces, C++, 贪心, 动态规划]
categories: CodeForces题解
cover: https://raw.githubusercontent.com/wsylab/wsyPicGo/main/img/photo-1497996377197-e4b9024658a4
description: CodeForces-1373D「Maximum Sum on Even Positions」的思考与解答
---
### [CodeForces-1373D Maximum Sum on Even Positions](https://codeforces.com/problemset/problem/1373/D)
### 题目大意
给定一个数列 $a_0, a_1, ..., a_{n - 1}$ , 你可以选择一段连续区间将其反转, 求能够得到的偶数位元素和的最大值.
### Solution 1
这道题我们只关心下标的奇偶性而不关心其具体的次序, 从这个角度思考反转操作带来的变化. 如果区间长度为奇数, 元素下标奇偶性不变, 对结果没有影响; 如果区间长度为奇数, 我们获得的收益就是奇下标元素和减去偶下标元素和. 根据区间最左侧元素下标的奇偶性分类讨论, 可以转化成差分数组 $d$ 的最大/最小子段和问题.
代码如下:
```C++
#include <bits/stdc++.h>
using namespace std;
long long get_max_seqsum(vector<long long> d) {
    int len = d.size();
    long long res = 0;
    long long sum = 0;
    for (int i = 0; i < len; i++) {
        sum += d[i];
        if (sum <= 0) {
            sum = 0;
        }
        else {
            res = max(res, sum);
        }
    }
    return res;
}

int main() {
    int t;
    cin>>t;
    while (t--) {
        int n;
        cin>>n;
        vector<long long> a(n, 0);
        long long sum = 0;
        long long ans = 0;
        for (int i = 0; i < n; i++) {
            cin>>a[i];
            if (i % 2 == 0) {
                sum += a[i];
            }
        }
        vector<long long> d_even, d_odd;
        for (int i = 1; i < n; i++) {
            if (i % 2 == 0) {
                d_odd.push_back(-(a[i] - a[i - 1]));
            }
            else {
                d_even.push_back(a[i] - a[i - 1]);
            }
        }
        cout<<sum + max(get_max_seqsum(d_odd), get_max_seqsum(d_even))<<endl;
    }
    return 0;
}
```