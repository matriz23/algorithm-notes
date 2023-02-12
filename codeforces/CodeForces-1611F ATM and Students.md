---
title: CodeForces-1611F ATM and Students 
date: 2022-08-26 13:28:00
updated:
tags: [计算机, 算法, CodeForces, C++]
categories: CodeForces题解
cover: https://raw.githubusercontent.com/wsylab/wsyPicGo/main/img/photo-1559496964-246917fbc294.avif
description: CodeForces-1611F「ATM and Students」的思考与解答
---
### [CodeForces-1611F ATM and Students](https://codeforces.com/problemset/problem/1611/F)
### 题目大意
给定长度为 $n$ 的数组 $a_1, a_2, ..., a_n$ 和一个数字 $s$ . 寻找 $a$ 最长的连续子数组 $b$ , 满足 $b$ 的任意前缀和均 $< -s$ . 如果 $b$ 存在, 输出其左右端点的下标; 否则输出 $-1$ .
### Solution 1
容易想到 $O(n^2)$ 的暴力做法, 枚举左端点, 对于每个左端点依次向右寻找右端点. 暴力解法中大量的实现耗费在了前缀和的反复计算上. 考虑优化这一点, 我们用 $sum$ 维护子数组的前缀和, 用 $r$ 记录子数组的右端点. 对于 $a[i]$ 开头的子数组, 如果 $a[i - 1]$ 开头的子数组不存在, 需要重新计算 $sum$ 和 $r$ . 否则, 如果 $a[i] >= 0$ , 那么一个正影响的元素从前缀和中被去除了, 向左缩小 $r$ ; 如果 $a[i] < 0$ , 那么一个负影响的元素从前缀和中被去除了, 向右扩大 $r$ . 期间维护最长的子数组长度以及对应下标即可.
代码如下:
```C++
#include <bits/stdc++.h>
using namespace std;
#define ll long long
int main() {
    int t;
    cin>>t;
    while (t--) {
        int n, s;
        cin>>n>>s;
        vector<ll> a(n + 1, 0);
        for (int i = 1; i <= n; i++) {
            cin>>a[i];
        }
        int max_len = 0;
        int ans_l = 0;
        int ans_r = 0;
        int r = -1;
        ll sum = 0;
        for (int i = 1; i <= n; i++) {
            if (r < i - 1) { // 上一个没法作为开头, 重新计算
                sum = 0;
                for (r = i; r <= n; r++) {
                    sum += a[r];
                    if (sum < -s) {
                        sum -= a[r];
                        r--;
                        break;
                    }
                }
                sum_idx = i;
                r = (r > n)? n: r;
            }
            else { // 如果上一个可以作为开头
                if (a[i - 1] >= 0) { // 正值, 退格
                    sum -= a[i - 1];
                    for (; r >= i - 1; ) {
                        if (sum < -s) {
                            sum -= a[r];
                            r--;
                        }
                        else {
                            break;
                        }
                    }
                    continue;
                }
                else if (a[i - 1] < 0) { // 负值, 向右扩展
                    sum -= a[i - 1];
                    r++;
                    for (; r <= n; r++) {
                        sum += a[r];
                        if (sum < -s) {
                            sum -= a[r];
                            r--;
                            break;
                        }
                    }
                    sum_idx = i;
                    r = (r > n)? n: r;
                }
            }
            if (r - i + 1 > max_len) {
                max_len = r - i + 1;
                ans_l = i;
                ans_r = r;
            }
        }
        if (ans_r == 0) {
            cout<<-1<<endl;
        }
        else {
            cout<<ans_l<<" "<<ans_r<<endl;
        }
    }
    return 0;
}
```
