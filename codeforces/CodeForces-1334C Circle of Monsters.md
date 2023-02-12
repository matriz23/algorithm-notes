---
title: CodeForces-1334C Circle of Monsters 
date: 2022-06-30 18:21:00
updated:
tags: [计算机, 算法, CodeForces, C++, 贪心]
categories: CodeForces题解
cover: https://raw.githubusercontent.com/wsylab/wsyPicGo/main/img/photo-1617398759584-bb7c6be1bf3c
description: CodeForces-1334C「Circle of Monsters」的思考与解答
---
### [CodeForces-1334C Circle of Monsters](https://codeforces.com/problemset/problem/1334/C)
### 题目大意
有 $N$ 头怪兽, 他们围成一个环, 顺时针编号 $1, 2, 3, 4,..., N$ 每一头怪兽都有 $2$ 个属性, 一个是它的生命值 $a_i$, 第二个是它的爆炸伤害 $b_i$.
你有一把手枪, 可以选中一个怪兽开一枪使其生命值 $-1$ .
当一个怪兽生命值 $\leq 0$ 时, 它会发生爆炸, 并对后一个怪兽造成 $b_i$ 点伤害. 爆炸可以连环发生, 但不会跨过已死的怪兽发生.
返回杀死所有怪兽所需要开的最少枪数.
### Solution 1
为了杀死一个怪兽, 我们可以选择用枪的伤害灌死它, 也可以考虑利用上一个怪兽爆炸的伤害来解决它, 显然后一种方法更省子弹. 为了尽可能多的利用爆炸伤害, 我们的目标就是达成一个理想状态, 使得一个怪兽爆炸后, 所有的怪兽都会连环爆炸. 为了达成这个状态, 我们先要开枪把怪兽的生命值降低到临界值, 如果 $b[i - 1] >= a[i]$ , 我们不需要额外开枪; 如果 $b[i - 1] < a[i]$ , 我们需要先开上 $a[i] - b[i - 1]$ 枪. 达成临界状态后, 需要一个"起爆点" , 我们选择临界值最小的怪兽开枪将其杀死即可.
这道题的测试数据卡`cin`, 需要用`scanf`代替.
代码如下:
```C++
#include <bits/stdc++.h>
using namespace std;
int main() {
    int t;
    scanf("%d", &t);
    while (t--) {
        long long ans = 0;
        int n;
        scanf("%d", &n);
        vector<long long> a(n, 0), b(n, 0);
        long long trigger = 1e12 + 10;
        for (int i = 0; i < n; i++) {
            scanf("%lld%lld", &a[i], &b[i]);
        }
        for (int i = 0; i < n; i++) {
            ans += max(a[i] - b[(i - 1 + n) % n], 0LL);
            a[i] -= max(a[i] - b[(i - 1 + n) % n], 0LL);
            trigger = min(trigger, a[i]);
        }
        ans += trigger;
        printf("%lld\n", ans);
    }
    return 0;
}
```
