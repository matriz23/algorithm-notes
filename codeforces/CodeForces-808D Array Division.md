---
title: CodeForces-808D Array Division 
date: 2022-08-04 15:18:00
updated:
tags: [计算机, 算法, CodeForces, C++, 二分, 哈希]
categories: CodeForces题解
cover: https://raw.githubusercontent.com/wsylab/wsyPicGo/main/img/photo-1524410411359-24e9a0aa7076
description: CodeForces-808D「Array Division」的思考与解答
---
### [CodeForces-808D Array Division](https://codeforces.com/problemset/problem/808/D)
### 题目大意
给定一个长度为 $n$ 的正整数数组 $a$ , 至多选择 $a$ 的一个元素移动到数组的任意位置, 如果能够使得存在一种分割方式使得 $a$ 的左右两部分和相等, 输出 $YES$ ; 否则输出 $NO$ .
### Solution 1
首先 $a$ 的和必须是偶数, 否则直接输出 $NO$ . 由于需要考虑左侧的区间和, 我们计算出 $a$ 的前缀数组 $pre$ . 如果 $\exist 0\leq i\leq n - 1, s.t.\ pre[i] = \frac{pre[n - 1]}{2}$ , 则不需要操作就可以达到目的, 输出 $YES$ . 下面考虑移动元素的情况. 如果把左侧的元素移动到右侧, 对于每个 $i$ , 我们考虑所有 $i\leq j < n$ , 查看是否有 $pre[j] - a_i = \frac{pre[n - 1]}{2}$ . 但直接这样做时间复杂度为 $O(n^2)$ , 无法通过本题. 根据数组元素均为正整数, $pre$ 数组单调增的特性, 我们可以用二分搜索来将时间复杂度减少至 $O(nlogn)$ . 把右侧元素移动的左侧的情况类似. 对于每个 $i$ , 考虑所有 $0\leq j < i$ , 查看是否有 $pre[j] + a_i =  \frac{pre[n - 1]}{2}$ 即可, 这一部分同样需要使用二分搜索优化.
代码如下:
```C++
#include <bits/stdc++.h>
using namespace std;
#define ll long long

int main() {
    int n;
    cin>>n;
    vector<ll> a(n, 0);
    vector<ll> pre(n, 0);
    for (int i = 0; i < n; i++) {
        cin>>a[i];
        pre[i] = (i == 0)? a[i]: pre[i - 1] + a[i];
    }
    if (pre[n - 1] % 2 == 1) {
        cout<<"NO"<<endl;
        return 0;
    }
    for (int i = 0; i < n; i++) {
        // 不需要移动元素
        if (pre[i] == pre[n - 1] / 2 ) {
            cout<<"YES"<<endl;
            return 0;
        }
        else {
            // 左侧元素移动至右侧
            int left = i;
            int right = n - 1; 
            while (left <= right) {
                int mid = left + (right - left) / 2;
                if (pre[mid] - a[i] == pre[n - 1] / 2) {
                    cout<<"YES"<<endl;
                    return 0;
                }
                else if (pre[mid] - a[i] < pre[n - 1] / 2) {
                    left = mid + 1;
                }
                else {
                    right = mid - 1;
                }
            }
            // 右侧元素移动至左侧
            left = 0;
            right = i - 1;
            while (left <= right) {
                int mid = left + (right - left) / 2;
                if (pre[mid] + a[i] == pre[n - 1] / 2) {
                    cout<<"YES"<<endl;
                    return 0;
                }
                else if (pre[mid] + a[i] < pre[n - 1] / 2) {
                    left = mid + 1;
                }
                else {
                    right = mid - 1;
                }
            }

        }
    }
    cout<<"NO"<<endl;
    return 0;
}
```

### Solution 2
我们二分搜索的过程可以使用哈希表代替. 使用两个哈希表分别存储左侧 (包括自身) 的数和右侧数, 判断是否能够通过移动元素将 $pre[i]$ 达到目标值 $\frac{pre[n - 1]}{2}$ .

```C++
#include <bits/stdc++.h>
using namespace std;
#define ll long long

int main() {
    int n;
    cin>>n;
    vector<ll> a(n, 0);
    vector<ll> pre(n, 0);
    map<ll, ll> l_map, r_map;
    for (int i = 0; i < n; i++) {
        cin>>a[i];
        r_map[a[i]]++;
        pre[i] = (i == 0)? a[i]: pre[i - 1] + a[i];
    }
    if (pre[n - 1] % 2 == 1) {
        cout<<"NO"<<endl;
        return 0;
    }
    for (int i = 0; i < n; i++) {
        l_map[a[i]]++; // 更新左侧 (包括自身) 的元素
        r_map[a[i]]--; // 更新右侧的元素
        if (r_map[a[i]] == 0) { // 右侧不存在 a_i 了, 清除
            r_map.erase(a[i]);
        }
        if (pre[i] == pre[n - 1] / 2) {
            cout<<"YES"<<endl;
            return 0;
        }
        else if (pre[i] < pre[n - 1] / 2) { // 不足, 从右侧补
            if (r_map.count(pre[n - 1] / 2 - pre[i])) {
                cout<<"YES"<<endl;
                return 0;
            }
        }
        else { // 超过, 从左侧删
            if (l_map.count(pre[i] - pre[n - 1] / 2)) {
                cout<<"YES"<<endl;
                return 0;
            }
        }
    }
    cout<<"NO"<<endl;
    return 0;
}
```