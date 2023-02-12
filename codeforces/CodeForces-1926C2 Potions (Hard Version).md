---
title: CodeForces-1926C2 Potions (Hard Version) 
date: 2022-06-30 08:35:00
updated:
tags: [计算机, 算法, CodeForces, C++, 贪心, 优先队列]
categories: CodeForces题解
cover: https://raw.githubusercontent.com/wsylab/wsyPicGo/main/img/photo-1612981499283-c98b772c0e3f
description: CodeForces-1926C2 「Potions (Hard Version)」的思考与解答
---
### [CodeForces-1926C2 Potions (Hard Version)](https://codeforces.com/problemset/problem/1526/C2)
### 题目大意
输入一个长度为 $n$ 的数组 $a$ , 求其前缀和恒非负的最长子序列(不要求连续)的长度. 
### Solution 1
首先遇到正元素一定要选择, 如果遇到负元素呢? 一个直接的想法是: 很小的不能选, 因为有可能因为选了这个很小的导致后面的一些负元素没办法选取. 如何把这种思考逻辑转化成策略? 我们使用贪心的策略, 遇到任何元素都选取, 当前缀和为负时, 再抛弃所选的最小元素, 这一部分使用优先队列维护即可. 显然这种选法是最优的, 并且能保证序列合法. 思考出贪心的关键点在于把一个依赖后续的决策变成了可以抛弃的贪心决策.
代码如下:
```C++
#include <bits/stdc++.h>
using namespace std;
int main() {
    int n;
    cin>>n;
    long long health = 0;
    int ans = 0;
    priority_queue<int, vector<int>, greater<int>> q;
    vector<int> a(n, 0);
    for (int i = 0; i < n; i++) {
        cin>>a[i];
        q.push(a[i]);
        health += a[i];
        ans++;
        if (health < 0) {
            int temp = q.top();
            q.pop();
            health -= temp;
            ans--;
        }
    }
    cout<<ans<<endl;
    return 0;
}
```