---
title: CodeForces-1398C Good Subarrays 
date: 2022-06-27 18:21:00
updated:
tags: [计算机, 算法, CodeForces, C++, 前缀和]
categories: CodeForces题解
cover: https://raw.githubusercontent.com/wsylab/wsyPicGo/main/img/photo-1507967669805-c23336beea5b
description: CodeForces-1398C「Good Subarrays」的思考与解答
---
### [CodeForces-1398C Good Subarrays](https://codeforces.com/problemset/problem/1398/C)
### 题目大意
如果一个数组满足元素之和等于数组长度, 则称这个数组为"好数组".
给定一个元素均为 $0$ 到 $9$ 之间整数的数组 $a_1, a_2,...,a_n$ , 返回其连续子数组中"好数组"的个数.
### Solution 1
注意到数组长度只和下标的相对位置有关系, 我们把题给数组从下标 $0$ 开始计数对答案没有影响. 考虑连续子数组 $a[l],...,a[r](l\leq r)$ , 若其为"好数组", 则应当有 $\sum_{k=l}^{r}a[k]=r - l + 1$ . 由于需要多次计算数组区间和, 使用前缀和进行优化, 记 $pre[i] = \sum_{k=0}^{i}a[k]$ , 约束改写为 $pre[r] - pre[l - 1] = r - l + 1$. 再记 $b[i] = pre[i] - i$ , 则约束简化为 $b[r] = b[l - 1]$ . 对于 $i = 0, 1,...,n - 1$, 可以计算出 $b[i]$ , 同时注意到当 $l = 0$ 时, 存在一个形式上的 $b[-1]$ , 其值为 $0 - (-1)=1$. 假设存在 $k_1,...,k_m$ 满足 $b[k_j]$值相等 , 则可以构成 $\frac{m × (m - 1)}{2}$ 个"好数组". 我们遍历 $b[i]$ 能取到的值, 把结果累加即可.
代码如下:
```C++
#include <bits/stdc++.h>
using namespace std;
int main() {
    int t;
    cin>>t;
    while (t--) {
        int n;
        string s;
        cin>>n>>s;
        long long ans = 0;
        vector<int> a(n, 0);
        map<long long, long long> book;
        for (int i = 0; i < n; i++) {
            a[i] = (i == 0)? s[i] - '0': s[i] - '0' + a[i - 1];
        }
        for (int i = 0; i < n; i++) {
            a[i] -= i;
            book[a[i]]++;
        }
        book[1]++; // a[-1] = 1
        auto it = book.begin();
        while (it != book.end()) {
            ans += (it->second - 1) * it->second / 2;
            it++;
        }
        cout<<ans<<endl;
    }
    return 0;
}
```

### Solution 2
有一种更容易理解的方法. 我们把 $a[i]$ 的每个元素都减去 $1$ , 则寻找和恰为 $0$ 的连续子数组即可. 从前向后计算前缀和, 每次计算前缀和时, 总个数便加上该数值之前出现过的次数. 如果前缀和为 $0$ , 则自身也能构成一个"好数组", 此时计数再 $+1$.
代码如下:
```C++
#include <bits/stdc++.h>
using namespace std;
int main() {
    int t;
    cin>>t;
    while (t--) {
        int n;
        string s;
        cin>>n>>s;
        long long ans = 0;
        vector<int> a(n, 0);
        map<long long, long long> book;
        for (int i = 0; i < n; i++) {
            a[i] = (i == 0)? s[i] - '0' - 1: s[i] - '0' - 1 + a[i - 1];
        }
        for (int i = 0; i < n; i++) {
            ans += book[a[i]];
            book[a[i]]++;
            if (a[i] == 0) {
                ans++;
            }
        }
        cout<<ans<<endl;
    }
    return 0;
}
```