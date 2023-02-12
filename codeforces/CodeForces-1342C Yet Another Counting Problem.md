---
title: CodeForces-1342C Yet Another Counting Problem 
date: 2022-07-20 14:20:00
updated:
tags: [计算机, 算法, 数学, CodeForces, C++, 数论]
categories: CodeForces题解
cover: https://raw.githubusercontent.com/wsylab/wsyPicGo/main/img/photo-1483729558449-99ef09a8c325
description: CodeForces-1342C「Yet Another Counting Problem」的思考与解答
---
### [CodeForces-1342C Yet Another Counting Problem](https://codeforces.com/problemset/problem/1342/C)
### 题目大意
给定正整数 $a, b, l, r$ , 查询满足 $x\in [l, r]$ 且 $(x\ mod\ a)\ mod\ b\not = (x\ mod\ b)\ mod\ a$ 的 $x$ 的个数.
### Solution 1
不妨先设 $a < b$ . 因为既要 $mod\ a$ 又要 $mod\ b$ , 我们考虑一个周期 $[0, lcm(a, b) - 1]$ . 对于 $x\in [0, lcm(a, b) - 1]$ , 需要 $x\ mod\ a\not = x\ mod\ b$ . 显然 $x\in [0, b - 1]$ 时是不满足条件的. 而对小数据试验后可以猜测 $\forall\ x\in [b, lcm(a, b) - 1]$ , 都有 $x\ mod\ a\not = x\ mod\ b$ . 下面我们来证明这一点. 
假设 $\exist\ x\in [b, lcm(a, b) - 1], s.t.\ x\ mod\ a = x\ mod\ b$ , 设 $x = kb + t, 0\leq t\leq a$ , 则 $t=x\ mod\ b = x\ mod\ a = kb\ mod\ a + t$, 有 $kb\ mod\ a=0$ . 由 $a|kb, b|kb$ , 可得 $lcm(a, b)|kb$ . 因为 $kb < x < lcm(a, b)$ , 所以 $kb = 0$ . 而 $x = kb + t = t < a < b$ 与 $x\in [b, lcm(a, b) - 1]$ 矛盾.
由上述证明可以得知, $x$ 在每个周期区间内数量均为 $lcm(a, b) - b$ . 统计 $[l, r]$ 区间时, 可以转化成 $[0, r]$ 与 $[0, l - 1]$ 区间的数目之差.
代码如下:
```C++
#include <bits/stdc++.h>
using namespace std;
int main() {
    int t;
    cin>>t;
    while (t--) {
        long long a, b, q;
        cin>>a>>b>>q;
        long long d = a / __gcd(a, b) * b;
        long long y = max(a, b);
        vector<long long> ans;
        while (q--) {
            long long res = 0;
            long long l, r;
            cin>>l>>r;
            res = ((r / d) * (d - y) + max(r % d - y + 1, (long long)(0))) - (((l - 1) / d) * (d - y) + max((l - 1) % d - y + 1, (long long)(0)));
            ans.push_back(res);
        }
        for (auto res: ans) {
            cout<<res<<" ";
        }
        cout<<endl;
    }
    return 0;
}
```