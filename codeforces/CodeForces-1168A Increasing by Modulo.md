---
title: CodeForces-1168A Increasing by Modulo 
date: 2022-09-12 16:24:00
updated:
tags: [计算机, 算法, CodeForces, C++, 二分, 贪心]
categories: CodeForces题解
cover: https://raw.githubusercontent.com/wsylab/wsyPicGo/main/img/photo-1662574428878-6969eba0b86c
description: CodeForces-1168A「Increasing by Modulo」的思考与解答
---
### [CodeForces-1168A Increasing by Modulo](https://codeforces.com/problemset/problem/1168/A)
### 题目大意
给定正整数 $n, m$ 和长度为 $n$ 的数组 $a$ . 每次操作中, 你可以选择数组的一些元素, 将元素 $x$ 变为 $(x + 1)\%m$ . 求让数组变为单调不减数组的最少操作次数.
### Solution 1
如果 $k$ 次操作可行, 那么 $k + 1$ 次操作一定也可行. 基于这一点我们可以二分搜索 $k$ . 从数据范围可以看出, $check$ 函数应当在线性时间内检验 $k$ 是否满足要求, 因此我们考虑贪心, 尽可能让结尾的数字最小. 注意到 $k$ 是对整个数组操作 $k$ 次, 对于每个元素, 我们分别可以操作 $k$ 次. 如果 $a[i] > a[i - 1]$ , 假设 $a[i]$ 能在 $k$ 次操作内到达 $a[i - 1]$ , 那么就变为 $a[i - 1]$ , 否则保留 $a[i]$ ; 如果 $a[i] = a[i - 1]$ , 保留 $a[i - 1]$ ; 如果 $a[i] < a[i - 1]$ , 尽可能增加到 $a[i - 1]$ , 如果不能, 则返回 `false` . 基于贪心我们找到了一个 $O(n)$ 时间的检验函数, 整体复杂度 $O(nlogm)$ .
代码如下:
```C++
#include <bits/stdc++.h>
using namespace std;

int n, m;
bool check(vector<int> a, int k) {
    a[0] = ((0 + m - a[0]) % m <= k)? 0: a[0];
    for (int i = 1; i < n; i++) {
        if (a[i] > a[i - 1]) {
            a[i] = ((a[i - 1] + m - a[i]) % m <= k)? a[i - 1]: a[i];
        }
        else if (a[i] == a[i - 1]) {
            continue;
        }
        else if (a[i] < a[i - 1]) {
            if (a[i - 1] - a[i] <= k) {
                a[i] = a[i - 1];
            }
            else {
                return false;
            }
        }
    }
    return true;
}

int main() {
    cin>>n>>m;
    vector<int> a(n, 0);
    for (int i = 0; i < n; i++) {
        cin>>a[i];
    }
    int left = 0, right = m;
    while (left < right) {
        int mid = left + (right - left) / 2;
        if (check(a, mid)) {
            right = mid;
        }
        else {
            left = mid + 1;
        }
    }
    cout<<left<<endl;
    return 0;
}
```