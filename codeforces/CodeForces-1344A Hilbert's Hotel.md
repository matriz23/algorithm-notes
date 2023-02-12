---
title: CodeForces-1344A Hilbert's Hotel 
date: 2022-06-30 22:48:00
updated:
tags: [计算机, 算法, CodeForces, C++, 数学]
categories: CodeForces题解
cover: https://raw.githubusercontent.com/wsylab/wsyPicGo/main/img/photo-1522653449448-2e107201a9a3
description: CodeForces-1344A「Hilbert's Hotel」的思考与解答
---
### [CodeForces-1344A Hilbert's Hotel](https://codeforces.com/problemset/problem/1344/A)
### 题目大意
给你一个无限长的数轴还有一个大小为 $n$ 的整数数组 $a_0,a_1...a_{n-1}$.
对于数轴上的所有表示整数的点按如下规则映射: 对于任意一个整数 $k$ , $k$ 将会被移动到 $k+a_{\,k  \bmod n}$ 所在的位置.
现在请你判断移动后是否有任意两个整数的位置相同, 输出 $"YES"$ 或 $"NO"$ .
### Solution 1
数字 $i$ 被映射到 $i + a[i \% n]$ , 注意到对不同 $n$ 长度周期内的数字映射结果都是平移 $n$ 的, 我们考虑一个周期内的像是否存在重复即可.
代码如下:
```C++
#include <bits/stdc++.h>
using namespace std;
int main() {
    int t;
    cin>>t;
    while (t--) {
        long long n;
        cin>>n;
        set<long long> st;
        long long temp = 0;
        for (long long i = 0; i < n; i++) {
            cin>>temp;
            st.insert(((i + temp) % n + n) % n);

        }
        if ((long long)st.size() == n) {
            cout<<"YES"<<endl;
        }
        else {
            cout<<"NO"<<endl;
        }
    }
    return 0;
}
```
