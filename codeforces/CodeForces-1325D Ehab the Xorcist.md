---
title: CodeForces-1325D Ehab the Xorcist 
date: 2022-08-25 09:51:00
updated:
tags: [计算机, 算法, CodeForces, C++, 构造, 贪心]
categories: CodeForces题解
cover: https://raw.githubusercontent.com/wsylab/wsyPicGo/main/img/photo-1505168125601-4ddfdea4c7e7.jpg
description: CodeForces-1325D「Ehab the Xorcist」的思考与解答
---
### [CodeForces-1325D Ehab the Xorcist](https://codeforces.com/problemset/problem/1325/D)
### 题目大意
给定正整数 $u, v$ , 试构造长度最短的数组, 使得数组的元素的异或和为 $u$ , 加和为 $v$ ; 如果不存在合法构造, 输出 $-1$ .
### Solution 1
不难发现异或的两个性质: 
- $x \bigoplus y \leq x + y$
- $x \bigoplus y\equiv x + y\ (mod\ 2)$

因此, 如果合法的构造存在, 一定有 $u\leq v$ 与 $u\equiv v\ (mod\ 2)$ . 事实上, 这也是个充分条件. 因为涉及到位运算, 我们从二进制的角度来考虑 $u, v$ , 可以通过把 $v$ 的一些高位拆分成低位来保证异或后的值与 $u$ 的对应位相同. 具体操作时, 用以下的贪心策略: 
- 如果 $v$ 第 $i$ 位 > $u$ 的第 $i$ 位, 那么把这一位取走 $1$ 向后拆;
- 如果 $v$ 第 $i$ 位 < $u$ 的第 $i$ 位, 那么向高位寻找第一个更大的位, 从这一位开始逐位向低位拆, 每次取走 $2$ 个.

因为 $u \leq v$ , 所以第二个操作中符合要求的高位一定是存在的. 把 $v$ 的每一位进行拆分后, 假设位的最大值为 $k$ (可以证明, 经过上述操作, $k\leq 3$) , 我们把 $u$ 拆分成 $k$ 个数即可. 此策略保证了这 $k$ 个数, 和为 $v$ , 每一位的异或和都与 $u$ 的对应位相同, 并且尽可能减少了位的最大值 (也就是最终构造的数组最短) , 因此是一个合法的策略.

代码如下:
```C++
#include <bits/stdc++.h>
using namespace std;
#define ll long long
const ll MOD = 1e9 + 7;
int main() {
    ll u, v;
    cin>>u>>v;
    if (u > v || (u - v) & 1) {
        cout<<-1<<endl;
        return 0;
    }
    vector<vector<int>> cnt(2, vector<int>(63, 0));
    for (int i = 0; i < 63; i++) {
        if (u & (ll)1 << i) {
            cnt[0][i]++;
        }
        if (v & (ll)1 << i) {
            cnt[1][i]++;
        }
    }
    for (int i = 59; i >= 1; i--) {
        if ((cnt[1][i] - cnt[0][i]) % 2 != 0) {
            if (cnt[1][i] < cnt[0][i]) {
                for (int j = i + 1; j < 63; j++) {
                    if (cnt[1][j] > cnt[0][j]) {
                        for (int k = j; k >= i + 1; k--) {
                            cnt[1][k] -= 2;
                            cnt[1][k - 1] += 4;
                        }
                        break;
                    }
                }
            }
            cnt[1][i] -= 1;
            cnt[1][i - 1] += 2;
        }
    }
    vector<ll> ans;
    for (int i = 0; i <= 5; i++) {
        ll res = 0;
        for (int j = 0; j < 63; j++) {
            if (cnt[1][j] != 0) {
                res += (ll)1 << j;
                cnt[1][j]--;
            }
        }
        if (res != 0) {
            ans.push_back(res);
        }
    }
    cout<<ans.size()<<endl;
    for (auto a: ans) {
        cout<<a<<" ";
    }
    cout<<endl;
    return 0;
}
```

### Solution 2
对于构造存在的情况, 我们考虑另外一种构造方式. 记 $d = v - u$ , 因为目标是把 $v$ 拆分成几部分, 异或和恰为 $u$ . 考虑直接把 $v$ 拆成 $u$ 加上一些和为 $d$ 的数, 只需要后面这一部分异或和为 $0$ 即可. 因为 $d$ 为偶数, 所以拆成 $v = u + \frac{d}{2} + \frac{d}{2}$ 就初步得到了一个构造. 还需要考虑的一点是, 如果 $u \& \frac{d}{2} = 0$ , 则 $u + \frac{d}{2}$ 不会发生进位, 有 $(u + \frac{d}{2}) \bigoplus \frac{d}{2}  = u\bigoplus \frac{d}{2}\bigoplus \frac{d}{2}$ , 数组长度可以缩减成 $2$ .
注意一些情况的特判. 
代码如下:
```C++
#include <bits/stdc++.h>
using namespace std;
#define ll long long
const ll MOD = 1e9 + 7;
int main() {
    ll u, v;
    cin>>u>>v;
    ll d = v - u;
    if (d < 0 || d & 1) {
        cout<<-1<<endl;
        return 0;
    }
    if (d == 0) {
        if (u == 0) {
            cout<<0<<endl;
        }
        else {
            cout<<1<<endl<<u<<endl;
        }
    }
    else {
        if (((d / 2) & u) == 0) {
            cout << 2 << endl;
            cout << ((d / 2) ^ u) << " " << d / 2 << endl;
        } else {
            cout << 3 << endl;
            cout << u << " " << d / 2 << " " << d / 2 << endl;
        }
    }
    return 0;
}
```