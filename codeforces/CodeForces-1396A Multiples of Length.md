---
title: CodeForces-1396A Multiples of Length 
date: 2022-07-09 14:43:00
updated:
tags: [计算机, 算法, CodeForces, C++, 数学, 构造]
categories: CodeForces题解
cover: https://raw.githubusercontent.com/wsylab/wsyPicGo/main/img/photo-1507936580189-3816b4abf640
description: CodeForces-1396A「Multiples of Length」的思考与解答
---
### [CodeForces-1396A Multiples of Length](https://codeforces.com/problemset/problem/1396/A)
### 题目大意
给你一个长度为 $n$ 的序列, 进行**恰好三次操作**，使得操作结束后序列所有数为 $0$ .
每次操作你将选择一个区间, 假设区间长度为$len$, 你可以这个区间的每个数加上 $len$ 的**任意倍**.
可以证明这个问题一定有解, 请你给出任意一个合法方案.
### Solution 1
恰好进行三次操作, 我们考虑极端一点的情况. 对所有元素进行操作, 可以同时加减 $n$ 的倍数, 如果序列已经满足都是 $n$ 的倍数, 我们这次操作可以使序列全部变为 $0$ . 在这之前还有两次操作, 我们需要把每个元素都变成 $n$ 的倍数. 选中一个长度为 $n - 1$ 的区间, 所有元素全部加上自身的 $n - 1$ 倍. 再单独选择剩下的那个元素, 加上自身的 $n - 1$ 倍即可.
注意 $n = 1$ 时需要特判.
代码如下:
```C++
#include <bits/stdc++.h>
using namespace std;
int main() {
    int n;
    cin>>n;
    vector<long long> a(n + 1, 0);
    for (int i = 1; i <= n; i++) {
        cin>>a[i];
    }
    if (n == 1) { // 特判
        cout<<1<<" "<<1<<endl;
        cout<<(long long)(-1) * a[1]<<endl;
        cout<<1<<" "<<1<<endl;
        cout<<0<<endl;
        cout<<1<<" "<<1<<endl;
        cout<<0<<endl;
        return 0;
    }
    // Step 1
    cout<<1<<" "<<n - 1<<endl;
    for (int i = 1; i <= n - 1; i++) {
        cout<<(long long)(n - 1) * a[i]<<" ";
    }
    cout<<endl;
    // Step 2
    cout<<n<<" "<<n<<endl;
    cout<<(long long)(n - 1) * a[n]<<endl;
    // Step 3
    cout<<1<<" "<<n<<endl;
    for (int i = 1; i <= n; i++) {
        cout<<(long long)(-1) * a[i] * n<<" ";
    }
    cout<<endl;
    return 0;
}
```

### 有关构造题的一些感想
0x3F大神说多做构造题有助于开拓思维. 这段时间在做CF上自己分段的题目, 发现CF构造题真的不少, 构造的标签通常和数学的标签一块出现. 看了不少解法, 对于高度抽象的题目, 还是要从极端的情况入手, 从具体的情况想是很难想到正解的.
CF上构造题的解法都很精妙, 令人拍案叫绝, 不管是用来学习还是欣赏, 都是很有价值的.