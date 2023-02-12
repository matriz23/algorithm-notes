---
title: CodeForces-1519D Maximum Sum of Products 
date: 2022-07-26 14:44:00
updated:
tags: [计算机, 算法, CodeForces, C++, 动态规划, 贪心]
categories: CodeForces题解
cover: https://raw.githubusercontent.com/wsylab/wsyPicGo/main/img/photo-1464211892349-8e50a8045e67
description: CodeForces-1519D「Maximum Sum of Products」的思考与解答
---
### [CodeForces-1519D Maximum Sum of Products](http://codeforces.com/problemset/problem/1519/D)
### 题目大意
给定长度为 $n$ 的正整数数组 $a_1, a_2,..., a_n$ 和 $b_1, b_2, ..., b_n$ . 可以选择 $a$ 的一段区间翻转, 求 $\sum_{i=1}^{n}a_i\times b_i$ 的可能最大值.

### Solution 1
对于一段区间 $[l, r]$ , 反转它带来的变化量为 $\sum_{i = l}^{r}a_i\times b_{l + r - i} - \sum_{i = l}^{r}a_i\times b_i =-\frac{1}{2} \sum_{i = l}^{r}(a_i - a_{l + r - i})\times (b_i - b_{l + r - i})$ , 注意到变化量的值只与首尾两两匹配的 $a_i, b_i$ 有关. 对于 $a_i, a_j, b_i, b_j$ , 只有 $(a_i - a_j)\times (b_i - b_j) < 0$ 时, 交换 $a_i, a_j$ 才是有利的. 这一点说明最终反转的区间 $[l, r]$ 必然满足 $(a_l - a_r)\times(b_l - b_r) < 0$ . 
注意到目标函数的对称性, 我们把 $l + r$ 相同的区间放到一块处理. 考虑区间 $[l_1, r_1]$ 与 $[l_2, r_2]$ , 有 $l_1 < l_2 < r_2 < r_1$ , 两者变化量之差为 $f[l_1, r_1] - f[l_2, r_2] = \sum_{i = l_1}^{l_2 - 1}-1\times(a_i - a_{l_1 + r_1 - i})\times(b_i - b_{l_1 + r_1 - i})$ , 可见这些区间中间的变化量是相同的, 区别在于外围的变化量. 我们从小到大枚举 $l$ , 计算 $f[l, r]$ 的过程中, 如果某个时刻外围累积变化量 $inc < 0$ , 那么之后会计算到的更小的区间一定是更优的, 直接停止当前的计算即可; 反之, 如果一个大区间被完整地计算了, 那么与其中心点相同的小区间一定不是最优的, 不需要再次计算. 优化之后的算法对于每个 $l + r$ 都只计算了最大的区间, 时间复杂度为 $O(n^2)$ .
代码如下:
```C++
#include "bits/stdc++.h"
using namespace std;
#define ll long long
int main() {
    int n;
    cin>>n;
    vector<ll> a(n, 0), b(n, 0);
    ll ap = 0;
    for (int i = 0; i < n; i++) {
        cin>>a[i];
    }
    for (int i = 0; i < n; i++) {
        cin>>b[i];
        ap += a[i] * b[i]; // 初始量
    }
    ll max_inc = 0;
    set<int> book; // 存储哪些 l + r 区间被计算过了
    for (int i = 0; i < n; i++) {
        for (int j = i + 1; j < n; j++) {
            if ((a[i] - a[j]) * (b[i] - b[j]) < 0 && !book.count(i + j)) {
                ll inc = 0;
                book.insert(i + j);
                for (int k = i; k <= j + i - k; k++) {
                    inc +=  (ll)(-1) * (a[k] - a[j + i - k]) * (b[k] - b[j + i - k]);
                    if (inc < 0) {
                        book.erase(i + j);
                        break;
                    }
                }
                max_inc = max(max_inc, inc);
            }
        }
    }
    cout<<ap + max_inc<<endl;
    return 0;
}
```