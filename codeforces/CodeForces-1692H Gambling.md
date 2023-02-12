---
title: CodeForces-1692H Gambling 
date: 2022-07-12 11:42:00
updated:
tags: [计算机, 算法, CodeForces, C++, 动态规划]
categories: CodeForces题解
cover: https://raw.githubusercontent.com/wsylab/wsyPicGo/main/img/photo-1608231883522-2efb1897a608
description: CodeForces-1692H「Gambling」的思考与解答
---
### [CodeForces-1692H Gambling](https://codeforces.com/contest/1692/problem/H)
### 题目大意
给定一个长度为 $n$ 的数列 $x$, 寻找 $a, l, r$ 使得 $\sum_{i = l}^r(I(x_i=a)-I(x_i\not = a))$ 最大.

### Solution 1
选定一个 $a$ , 可以把等于 $a$ 的数看成 $1$ , 不等于 $a$ 的数看成 $-1$ , 则问题转化成了经典的"最大子段和"问题, 我们采取如下策略: 不断向右扩展, 如果区间和 $< 0$ 就舍弃当前区间重新开始, 否则延展区间. 在扩展区间时一定是贪心的, 必须有区间的左右端点都是 $a$ . 基于这一点, 在数据读取阶段用一个哈希表记录各个值的坐标, 再对每个可能的值 $a$ 进行上述处理即可.
代码如下:
```C++
#include <bits/stdc++.h>
using namespace std;
int main() {
    vector<int> test;
    int t;
    cin>>t;
    while (t--) {
        int n;
        cin>>n;
        vector<int> a(n + 1, 0);
        map<int, vector<int>> idx;
        for (int i = 1; i <= n; i++) {
            cin>>a[i];
            idx[a[i]].push_back(i);
        }
        int maxVal = 1; // 记录当前最大得分
        int ansNum = a[1]; // 记录当前最大得分对应的数字
        int ansLeft = 1; // 记录当前最大得分对应的区间左端点
        int ansRight = 1; // 记录当前最大得分对应的区间右端点
        for (auto it = idx.begin(); it != idx.end(); it++) {
            int sum = 1; // 初始化num的得分, 至少是1
            int left = (it->second)[0];
            int right = left;
            int num = it->first;
            for (int j = 1; j < (it->second).size(); j++) { // 不断向右扩展
                int temp = (it->second)[j]; // 新的右端点
                sum = sum - (temp - right - 1) + 1; // 新得分
                right = temp; // 右端点变化
                if (sum <= 0) { // 新得分<0, 舍弃
                    sum = 1;
                    left = temp; // 当前左端点右移
                    // right = left;
                }
                else { // 维持左端点, 仅仅变化了右端点
                    if (sum > maxVal) { // 最好结果更新
                        ansLeft = left;
                        ansRight = right;
                        ansNum = num;
                        maxVal = sum;
                    }
                }
            }
        }
        cout<<ansNum<<" "<<ansLeft<<" "<<ansRight<<endl;
    }
    return 0;
}
```