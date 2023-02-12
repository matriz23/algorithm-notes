---
title: CodeForces-439C Devu and Partitioning of the Array 
date: 2022-07-15 12:04:00
updated:
tags: [计算机, 算法, CodeForces, C++]
categories: CodeForces题解
cover: https://raw.githubusercontent.com/wsylab/wsyPicGo/main/img/photo-1531302271066-91cdfbac3508
description: CodeForces-439C「Devu and Partitioning of the Array」的思考与解答
---
### [CodeForces-439C Devu and Partitioning of the Array]()
### 题目大意
给定 $n, k, p$ 和一个数组 $a_1, a_2, ..., a_n$ , 如果能把数组分成 $k$ 个数组(不要求连续), 使得其中 $p$ 个和为偶数, $k - p$ 个和为奇数, 则输出 $YES$ 并给出具体方案, 否则输出 $NO$ .
### Solution 1
单独 $1$ 个奇数构成一个奇数数组, 每 $2$ 个奇数构成一个偶数数组. 首先奇数的个数要多于目标奇数数组的个数, 偶数的个数 $+$ 多余奇数对的个数要多余目标偶数数组的个数, 之后分配即可.
这道题需要考虑好各种细节情况.
代码如下:
```C++
#include <bits/stdc++.h>
using namespace std;
int main() {
    int n, k, p;
    cin>>n>>k>>p;
    int q = k - p;
    vector<int> odd_idx, even_idx;
    vector<int> a(n + 1, 0);
    for (int i = 1; i <= n; i++) {
        cin>>a[i];
        if (a[i] % 2 == 0) {
            even_idx.push_back(i);
        }
        else {
            odd_idx.push_back(i);
        }
    }
    int odd_num = odd_idx.size();
    int even_num = even_idx.size();
    if (odd_num < q) {
        cout<<"NO"<<endl;
        return 0;
    }
    int odd_res = odd_num - q;
    if (odd_res % 2 != 0) {
        cout<<"NO"<<endl;
        return 0;
    }

    int even_new = odd_res / 2;
    if (even_num + even_new < p) {
        cout<<"NO"<<endl;
        return 0;
    }
    cout<<"YES"<<endl;
    if (p != 0) {
        for (int i = 0; i < q; i++) {
            cout<<1<<" "<<a[odd_idx[i]]<<endl;
        }
        if (even_num >= p) {
            for (int i = 0; i < p - 1; i++) {
                cout<<1<<" "<<a[even_idx[i]]<<endl;
            }
            cout<<(even_num - p + 1 + odd_res)<<" ";
            for (int i = p - 1; i < even_num; i++) {
                cout<<a[even_idx[i]]<<" ";
            }
            for (int i = q; i < odd_num; i++) {
                cout<<a[odd_idx[i]]<<" ";
            }
            cout<<endl;
        }
        else {
            int vac = p - even_num;
            for (int i = 0; i < even_num; i++) {
                cout<<1<<" "<<a[even_idx[i]]<<endl;
            }
            for (int i = q; i <= q + 2 * (vac - 2) ; i += 2) {
                cout<<2<<" "<<a[odd_idx[i]]<<" "<<a[odd_idx[i + 1]]<<endl;
            }
            cout<<odd_num - (q + 2 * (vac - 2) + 2)<<" ";
            for (int i = q + 2 * (vac - 2) + 2; i < odd_num; i++) {
                cout<<a[odd_idx[i]]<<" ";
            }
            cout<<endl;
        }
    }
    else {
        for (int i = 0; i < q - 1; i++) {
            cout<<1<<" "<<a[odd_idx[i]]<<endl;
        }
        cout<<odd_num - q + 1 + even_num<<" ";
        for (int i = q - 1; i < odd_num; i++) {
            cout<<a[odd_idx[i]]<<" ";
        }
        for (int i = 0; i < even_num; i++) {
            cout<<a[even_idx[i]]<<" ";
        }
        cout<<endl;
    }
    return 0;
}
```

> ![](https://raw.githubusercontent.com/wsylab/wsyPicGo/main/img/20220715121105.png)
> 坑真的不少...