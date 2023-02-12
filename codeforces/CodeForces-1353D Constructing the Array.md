---
title: CodeForces-1353D Constructing the Array 
date: 2022-06-30 05:16:00
updated:
tags: [计算机, 算法, CodeForces, C++, 数据结构, 优先队列]
categories: CodeForces题解
cover: 
description: CodeForces-1353d「Constructing the Array」的思考与解答
---
### [CodeForces-1353D Constructing the Array](https://codeforces.com/problemset/problem/1353/D)

### 题目大意
输入一个 $n$ , 对一个长度为 $n$ 的全 $'0'$ 串进行 $n$ 次操作，第 $i$ 次操作选中当前长度最长的全 $'0'$ 子串中最靠左的, 记其左右端点坐标分别为 $left, right$. 如果 $right - left + 1$ 是奇数，那么第 $\frac{left + right}{2}$ 个数更新为 $i$ ，否则第 $\frac{left + right - 1}{2}$ 个数更新成 $i$ . 输出 $n$ 次操作后的串.
### Solution 1
想办法维护每次需要的取出的区间, 这一点通过优先队列来实现. 定义结构体存储左右端点, 重载运算符, 按照区间大小, 左端点前后排序. 之后模拟题给操作即可.
代码如下:
```C++
#include <bits/stdc++.h>
using namespace std;
struct node {
    int left, right;
    bool operator < (const node &y) const {
        int len1 = right - left + 1, len2 = y.right - y.left + 1;
        return len1 == len2? (left > y.left): (len1 < len2);
    }
};

int main() {
    int t;
    cin>>t;
    while (t--) {
        int n;
        cin>>n;
        vector<int> a(n + 1, 0);
        priority_queue<node> q;
        q.push((node){1, n});
        for (int i = 1; i <= n; i++) {
            node temp = q.top();
            q.pop();
            a[(temp.left + temp.right) / 2] = i;
            if ((temp.left + temp.right) / 2 - temp.left >= 1) {
                q.push(node{temp.left, (temp.left + temp.right) / 2 - 1});
            }
            if (temp.right - ((temp.left + temp.right) / 2) >= 1) {
                q.push(node{(temp.left + temp.right) / 2 + 1, temp.right});
            }
        }
        for (int i = 1; i <= n; i++) {
            cout<<a[i]<<" ";
        }
        cout<<endl;
    }
    return 0;
}
```