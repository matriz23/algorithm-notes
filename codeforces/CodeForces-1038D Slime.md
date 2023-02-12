---
title: CodeForces-1038D Slime 
date: 2022-07-19 18:22:00
updated:
tags: [计算机, 算法, CodeForces, C++, 构造, 贪心]
categories: CodeForces题解
cover: https://raw.githubusercontent.com/wsylab/wsyPicGo/main/img/photo-1602429438429-3def765b84d4
description: CodeForces-1038D「Slime」的思考与解答
---
### [CodeForces-1038D Slime](https://codeforces.com/contest/1038/problem/D)
### 题目大意
有 $n$ 个史莱姆排成一列, 每个史莱姆的价值为 $a_i$ . 史莱姆可以吞噬它相邻的史莱姆, 并让自身价值减去被吞噬者的价值. 经过 $n - 1$ 次吞噬后, 只剩下一个史莱姆, 求其可能的最大价值. 
### Solution 1
执行的都是"减"的运算, 一个常规的思考模式是, 对于负数我们尽可能减去, 对于一个正数, 我们尽可能减去一个减去了它的数(负负得正). 关键是如何最大化这一操作收益呢? 由于我们至少要减掉一个数, 可以估计上界是由 $n$ 个值添加至少一个负号后得到的. 也就是说, 减去一个最小值之后, 对于剩下的数, 可以加上正数, 减去负数, 得到最大得分. 下面我们通过构造证明这一操作总是可行的.
考虑连续的一段正数, 假如其左右侧不存在负数(整个数组都是非负的), 则有最小值依次减去周围值, 再由两侧任意一个数减掉这个最小值即可. 如正数段一侧存在负数, 用负数减去正数段可以把正数段并入负数中, 如此合并直到剩下最后一个正数段, 我们留一个正数, 其余合并到负数里, 再用这个正数减去所有负数即可. 两种情况下都能实现最优操作.
具体实现时, 可以在读入数据的过程中计算绝对值和与最大值、最小值. 如果最大值为负, 则计算的结果中多了两倍的最大值绝对值. 如果最小值为正, 则计算的结果中多了两倍的最小值, 修正后可以得到答案.
$n = 1$ 时需要特判, 我们无法进行任何操作, 直接返回答案.
代码如下:
```C++
#include <bits/stdc++.h>
using namespace std;
int main() {
    int n;
    cin>>n;
    if (n == 1) {
        int ans = 0;
        cin>>ans;
        cout<<ans<<endl;
        return 0;
    }
    long long maxnum = -(1e9 + 7);
    long long minnum = 1e9 + 7;
    long long ans = 0;
    long long temp = 0;
    while (n--) {
        cin>>temp;
        ans += max(temp, -1 * temp);
        maxnum = max(maxnum, temp);
        minnum = min(minnum, temp);
    }
    if (maxnum < 0) {
        ans += (long long)(2) * maxnum;
    }
    if (minnum > 0) {
        ans -= (long long)(2) * minnum;
    }
    cout<<ans<<endl;
    return 0;
}
```