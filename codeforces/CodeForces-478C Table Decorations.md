---
title: CodeForces-478C Table Decorations 
date: 2022-07-25 15:30:00
updated:
tags: [计算机, 算法, CodeForces, C++, 贪心]
categories: CodeForces题解
cover: https://raw.githubusercontent.com/wsylab/wsyPicGo/main/img/photo-1504196606672-aef5c9cefc92
description: CodeForces-478C「Table Decorations」的思考与解答
---
### [CodeForces-478C Table Decorations](http://codeforces.com/problemset/problem/478/C)
### 题目大意
你正在为宴会装饰桌子, 一张桌子需要用 $3$ 个气球装饰, 且这 $3$ 个气球颜色不能是同一种颜色. 给你 $r$ 个红色气球, $g$ 个绿色气球, $b$ 个蓝色气球, 求最多能装饰的桌子数.

### Solution 1
我们可以用 $abb$ 和 $abc$ 两种模式来装饰桌子. 一个朴素的想法是, 三种颜色的气球数量差不多时, 尽可能均摊, 即用 $abc$ 模式来装饰桌子. 而有一种气球数量特别多时, 优先使用 $abb$ 模式显然更合理. 
不妨设 $r\leq g\leq b$ , 如果有 $b\geq 2\times(r + g)$ , 由于一次装饰至少消耗掉一个 $r$ 或 $g$ , 至多装饰 $r + g$ 个桌子, 我们可以用 $rbb$ 和 $gbb$ 来达到这个上界. 
如果 $b < 2\times(r + g)$ , 我们仍然可以一次使用两个 $b$ , 但在某一时刻, $b$ 的数量会和 $r, g$ 很接近. 考虑最终的两种形态: $t, t, b^*\ (b^* \leq t + 2)$ , 以及 $t, t + 1, t + 1$ . 因为 $t\leq b^* = b - 2\times(r - t) - 2\times(g - t)\leq  t + 2$ , 有 $\frac{2r + 2g - b}{3}\leq t\leq \frac{2r + 2g - b + 2}{3}$ . 判断 $t$ 是否满足 $t \leq r$ , 如果满足, 则总桌子数为 $(r - t) + (b - t) + t$ . 如果不满足, 说明无法转化到 $t, t, b^*$ , 考虑转化到 $t, t + 1, t + 1$ . 同样计算约束, 有 $t = \frac{2r + 2g - b - 1}{3}$ , 此时总桌子数为 $(r - t) + (g - (t + 1)) + t$ .
代码如下:
```C++
#include "bits/stdc++.h"
using namespace std;
#define ll long long
int main() {
    vector<ll> a(3, 0);
    cin>>a[0]>>a[1]>>a[2];
    sort(a.begin(), a.end());
    if (a[2] >= 2 * (a[0] + a[1])) {
        cout<<a[0] + a[1]<<endl;
        return 0;
    }
    ll res = 0;
    ll t = (2 * a[0] + 2 * a[1] - a[2] + 2) / 3;
    if (t <= a[0]) {
        res = (a[0] - t) + (a[1] - t) + t;
    }
    else {
        t = (2 * a[0] + 2 * a[1] - a[2] - 1) / 3;
        res = (a[0] - t) + (a[1] - (t + 1)) + t;
    }
    cout<<res<<endl;
    return 0;
}
```

### Solution 2
对于 $b\geq 2\times(r + g)$ 的处理不变, 考虑 $b< 2\times(r + g)$ 时, 因为 $1$ 个桌子用掉 $3$ 个气球, 数量上的约束使得答案不超过 $\lfloor \frac {r + g + b}{3}\rfloor$ . 事实上这个上界能取到. 考虑 **Solution 1** 中的分析过程, 不管化为 $t, t, b^*\ (b^* \leq t + 2)$ 还是 $t, t + 1, t + 1$ , 最终没有用上的气球都至多是 $2$ 个, 其余的气球全部用上了, 答案自然是 $\lfloor \frac {r + g + b}{3}\rfloor$ .
代码如下:
```C++
#include "bits/stdc++.h"
using namespace std;
#define ll long long
int main() {
    vector<ll> a(3, 0);
    cin>>a[0]>>a[1]>>a[2];
    sort(a.begin(), a.end());
    if (a[2] >= 2 * (a[0] + a[1])) {
        cout<<a[0] + a[1]<<endl;
    }
    else {
        cout<<(a[0] + a[1] + a[2]) / 3<<endl;
    }
    return 0;
}
```