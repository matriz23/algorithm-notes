---
title: CodeForces-1338A Powered Addition 
date: 2022-06-23 13:24:00
updated:
tags: [计算机, 算法, CodeForces, C++, 逆序对, 二进制]
categories: CodeForces题解
cover: https://raw.githubusercontent.com/wsylab/wsyPicGo/main/img/photo-1440470177828-6381dc5074ba
description: CodeForces-1338A「Powered Addition 」的思考与解答
---
### [CodeForces-1338A Powered Addition](https://codeforces.com/problemset/problem/1338/A)

### 题目大意
输入正整数 $t$ , 一共有 $t$ 个测试点, 每个测试点输入 $n$ , 之后输入 $n$ 个整数代表 $a_0, a_1,...,a_{n-1}$ . 在 $x$ 时刻, 可以任意选择 $a$ 中元素使其增加 $2^{x-1}$ . 返回使得 $a$ 为单调不减数列所需的最少时间.
### Solution 1
我们从左向右查看数列中的元素, 对于前 $i + 1$ 个数, 需要满足单调不减, 则需要 $a_i$ 增加到 $\underset{0\leq k\leq i}{max}\ a_k$ , 从二进制的角度考虑, 每一次至多改变差值上的一位, $\underset{0\leq k\leq i}{max}\ a_k -a_i$ 的位数就是处理前 $i + 1$ 个数所需的时间. 注意到这一操作对后续求解没有影响, 我们所求即是 $\underset{0\leq i\leq n-1}{max}(\underset{0\leq k\leq i}{max}\ a_k-a_i)$ 的位数.
代码如下:
```C++
#include <bits/stdc++.h>
using namespace std;
int main() {
    int t;
    cin>>t;
    while (t--) {
        int n;
        cin>>n;
        vector<int> a(n, 0);
        int ans = 0;
        int nowmax = -1000000007;
        int gap = 0;
        for (int i = 0; i < n; i++) {
            cin>>a[i];
            nowmax = max(nowmax, a[i]);
            gap = max(gap, nowmax - a[i]);
        }
        while (gap) {
            gap>>=1;
            ans++;
        }
        cout<<ans<<endl;
    }
    return 0;
}
```