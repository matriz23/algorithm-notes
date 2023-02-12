---
title: CodeForces-1015D Walking Between Houses 
date: 2022-07-12 15:57:00
updated:
tags: [计算机, 算法, CodeForces, C++, 构造, 贪心]
categories: CodeForces题解
cover: https://raw.githubusercontent.com/wsylab/wsyPicGo/main/img/photo-1464082354059-27db6ce50048
description: CodeForces-1015D「Walking Between Houses」的思考与解答
---
### [CodeForces-1015D Walking Between Houses](https://codeforces.com/contest/1015/problem/D)
### 题目大意
在一条路上, $n$ 个房子被排成一排, 从左到右编号为 $1-n$ . 一开始，你站在 $1$ 号房子前.
你需要移动到其他的房子恰好 $k$ 次. 每一次移动必须从 $x$ 房子移动到 $y$ 房子( $x\not = y$), 走过的距离为 $\left| x-y \right|$. 可以多次到达同一个房子. 你的目标是一共走 $s$ 个单位长度.
如果是不可能的, 输出 $NO$ ; 否则输出 $YES$ , 并输出任意一种可行的方案.

### Solution 1
我们每一步最多走 $n - 1$ 步(从一个端点到另一个端点), 最少走 $1$ 步. 初步估计可行的范围是 $[k, k × (n - 1)]$ . 如果 $s$ 不在这个范围里, 直接输出 $NO$ 即可. 如果 $s$ 在这个范围里, 我们可以证明一定存在合法的方案. 
构造可行方案时, 我们可以借鉴显微镜粗细准焦螺旋的思路: 先走大的, 再走小的. 首先初始化走最大步的步数 $cnt = \lfloor \frac{s}{n - 1}\rfloor$ , 此时剩余步数为 $step = k - cnt$ , 剩余距离为 $res = s - cnt × (n - 1)$ , 如果方案可行必须有 $res < step$ , 我们不断从 $cnt$ 中减少步数分配到 $step$ 中, 直到有 $res >= step$ . 这时尽量走最小的步数, 直到最后一步把剩余的走掉.
代码如下:
```C++
#include <bits/stdc++.h>
using namespace std;
int main() {
    long long n, k, s;
    cin>>n>>k>>s;
    if (k * (n - 1) < s || k * 1 > s) {
        cout<<"NO"<<endl;
        return 0;
    }
    cout<<"YES"<<endl;
    long long step = k;
    long long cnt = s / (n - 1);
    step -= cnt;
    long long res = s - cnt * (n - 1);
    while (res < step) {
        cnt--;
        res += (n - 1);
        step++;
    }
    long long pos = 1;
    for (int i = 1; i <= cnt; i++) {
        if (i % 2 == 1) {
            pos = n;
            cout<<n<<" ";
        }
        else {
            pos = 1;
            cout<<1<<" ";
        }
    }
    for (int i = 1; i < step; i++) {
        if (i % 2 == 1) {
            if (pos == n) {
                pos = n - 1;
                cout<<n - 1<<" ";
            }
            else {
                pos = 2;
                cout<<2<<" ";
            }
        }
        else {
            if (pos == n - 1) {
                pos = n;
                cout<<n<<" ";
            }
            else {
                pos = 1;
                cout<<1<<" ";
            }
        }
        res--;
    }
    if (res != 0) {
        if (pos + res > n) {
            cout<<pos - res<<endl;
        }
        else {
            cout<<pos + res<<endl;
        }
    }
    return 0;
}
```
> 这里补充一个点, 我自己写代码时也没有考虑清楚, 就是 `if (res = 0)` 的这个判断框, 会不会导致一共只走了 $cnt + (step - 1)$ 步呢? 其实不会, 我们在处理 $cnt$ 时就已经保证了 $res\geq step$ , 所以如果 $res = 0$ 的情况发生了, 说明 $step$ 的循环根本没有走, 也就只有 $cnt = k$ 一部分构成决策方案. 这一点是我后来才注意到的, 写代码还是要仔细.
### Solution 2
在处理步数时, 我们也可以先计算平均步数 $\lfloor \frac{s}{k}\rfloor$ , 剩余的步数 $s\ mod\ k$ 均匀分配即可.
代码见洛谷用户Allanljx的博客文章[CF1015D题解](https://www.luogu.com.cn/blog/406947/solution-cf1015d).