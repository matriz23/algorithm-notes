---
title: CodeForces-1718A2 Burenka and Traditions (hard version) 
date: 2023-01-08 21:22:00
updated:
tags: [计算机, 算法, CodeForces, C++, 贪心]
categories: CodeForces题解
cover: 
description: CodeForces-1718A2「Burenka and Traditions (hard version)」的思考与解答
---
### [CodeForces-1718A2 Burenka and Traditions (hard version)](https://codeforces.com/problemset/problem/1718/A2)
### 题目大意
给定长度为 $n$ 的数组 $a_1, a_2, ..., a_n (0\leq a_i<2^{30})$ . 每次操作可以将 $a_l, ..., a_r$ 全部异或某个数, 代价为 $\lfloor \frac{r - l + 1}{2}\rfloor$ . 求使得 $a$ 元素全变为 $0$ 的最小代价.
### Solution 1
操作的代价与区间长度有关, 当区间长度为 $2k$ 时, 代价为 $k$ , 同样的代价可以对 $k$ 个长度为 $2$ 的区间分别进行操作; 当区间长度为 $2k + 1$ 时, 代价为 $k + 1$ , 同样的代价可以操作 $1$ 个长度为 $1$ 的区间和 $k$ 个长度为 $2$ 的区间. 因此, 最优的操作应当由代价为 $1$ 的操作构成.
如果对全部元素进行单元素操作, 那么总代价为 $n$ . 我们需要寻找的就是可以合并操作的元素. 对于一个异或和为 $0$ 的子区间, 可以用区间长度 $-1$ 的代价把区间变为 $0$ . 因此, 统计原数组中最多有多少个不相交的子区间异或和为 $0$ 即可, 最终答案即为 $n-$ 减去这个数. \
可以利用前缀和以及哈希表完成统计.
代码如下:
```C++
#include <bits/stdc++.h>

using namespace std;

int main() {
    int t;
    cin >> t;
    while (t--) {
        int n;
        cin >> n;
        int a[n];
        for (int i = 0; i < n; i++) {
            cin >> a[i];
        }
        int ans = n;
        set<int> s;  
        int xs = 0;
        for (int v : a) {
            xs = xs ^ v;
            if (s.count(xs) || xs == 0) { // 前缀和之差的区间 或 前缀和区间
                ans--;
                s.clear(); // 为了统计不相交的区间个数, 把哈希表重置
                xs = 0; // 前缀和重置
            }
            s.insert(xs);
        }
        cout << ans << endl;
    }
    return 0;
}
```