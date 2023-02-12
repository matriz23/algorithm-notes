---
title: CodeForces-1692G 2^Sort 
date: 2022-07-12 10:29:00
updated:
tags: [计算机, 算法, CodeForces, C++]
categories: CodeForces题解
cover: https://raw.githubusercontent.com/wsylab/wsyPicGo/main/img/photo-1504173010664-32509aeebb62
description: CodeForces-1692G「2^Sort」的思考与解答
---
### [CodeForces-1692G 2^Sort](https://codeforces.com/contest/1692/problem/G)

### 题目大意
输入一个为 $n$ 的数组 $a$, 计算在这个数组中, 长度为 $k + 1 \ (1\leq k < n)$ 且符合以下条件的区间个数:
$$
2^0 × a_i < 2^1 × a_{i + 1} < 2^2 × a_{i + 2} < \dotsi < 2^k × a_{i + k}\\
\footnotesize{注：i 为这个区间开始的位置}
$$

### Solution 1
对于每个 $a_i$ , 我们搜索以其为右端点的合法序列, 已经搜过的不用再搜, 对于长度为 $cnt > k$ 的序列, 能够产生 $cnt - k$ 个合法子序列. 为了方便操作, 我们把序列左侧加入一个 $INT\_MAX$ , 这个数能够把边界情况考虑进去. 
代码如下:
```C++
#include <bits/stdc++.h>
using namespace std;
int main() {
    int t;
    cin>>t;
    while (t--) {
        int n, k;
        cin>>n>>k;
        vector<int> a(n + 1, 0);
        a[0] = INT_MAX;
        for (int i = 1; i <= n; i++) {
            cin>>a[i];
        }
        int ans = 0;
        int left = n;
        int right = n;
        while (left >= 1) {
            int cnt = 1;
            int bd = a[right];
            while (a[left - 1] < bd * 2) {
                left--;
                bd = a[left];
                cnt++;
            } // 有点类似单调栈
            ans += (cnt > k)? (cnt - k): 0;
            right = left - 1;
            left = left - 1;
        }
        cout<<ans<<endl; 
    }
    return 0;
}
```

## Solution 2
用 $bool$ 数组记录相邻元素是否满足大小要求, 最后在 $bool$ 数组上统计. 同样的我们增加的一个哨兵元素, 防止有元素在栈里没有弹出.
代码如下:
```C++
#include <bits/stdc++.h>
using namespace std;
int main() {
    vector<int> test;
    int t;
    cin>>t;
    while (t--) {
        int n, k;
        cin>>n>>k;
        vector<int> a(n, 0);
        for (int i = 0; i < n; i++) {
            cin>>a[i];
        }
        int ans = 0, cnt = 0;
        vector<bool> b(n, false);
        for (int i = 1; i < n; i++) {
            if (a[i - 1] < 2 * a[i]) {
                b[i - 1] = true; 
            }
        }
        for (int i = 0; i < n; i++) {
            if (b[i]) {
                cnt++;
            }
            else {
                ans +=  (cnt >= k)? (cnt - k + 1): 0;
                cnt = 0;
            }
        }
        cout<<ans<<endl;
    }
    return 0;
}
```