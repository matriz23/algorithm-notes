---
title: CodeForces-1081B Farewell Party 
date: 2022-07-15 14:29:00
updated:
tags: [计算机, 算法, CodeForces, C++, 构造]
categories: CodeForces题解
cover: https://raw.githubusercontent.com/wsylab/wsyPicGo/main/img/photo-1513151233558-d860c5398176
description: CodeForces-1081B「Farewell Party」的思考与解答
---
### [CodeForces-1081B Farewell Party]()
### 题目大意
有 $n$ 个人参加了一个派对, 每个人带着一顶标有 $1, 2, ..., n$ 中一个数的帽子, $a_i$ 是 $i$ 号人口中和他自己帽子不一样的人数. 输入 $n$ 和数组 $a$ , 判断这种情况是否可能发生. 如果可能, 输出 $Possible$ 并且给出一种合法的帽子的分布; 否则输出 $Impossible$ .
### Solution 1
我们记 $b_i = n - a_i$ , 则 $b_i$ 代表与 $i$ 号人帽子相同的人数(包括他自己) , 这些人对应的 $b$ 值也应该与 $b_i$ 相等. 我们根据 $b_i$ 的值就可以把人划分成许多类, 每一类中也可能有不同的帽子, 只不过恰好 $b_i$ 值相同, 这要求满足 $b_i = k$ 的元素 $i$ 的个数应该是 $k$ 的整数倍. 根据这一点我们分配具体的帽子数值即可.
代码如下:
```C++
#include <bits/stdc++.h>
using namespace std;
int main() {
    int n;
    cin>>n;
    map<int, int> book;
    vector<int> a(n, 0);
    for (int i = 0; i < n; i++) {
        cin>>a[i];
        book[n - a[i]]++; //根据b_i值统计人数
    }
    for (auto it = book.begin(); it != book.end(); it++) {
        if (it->second % it->first != 0) { // 人数应当是b_i的整数倍
            cout << "Impossible" << endl;
            return 0;
        }
    }
    cout<<"Possible"<<endl;
    int num = 1;
    map<int, vector<int>> idx;
    for (auto it = book.begin(); it != book.end(); it++) {
        for (int i = 0; i < it->second / it->first; i++) {
            for (int j = 0; j < it->first; j++) {
                idx[it->first].push_back(num); // 记录一个b_i值对应的帽子号码
            }
            num++;
        }
    }
    for (int i = 0; i < n; i++) {
        cout<<idx[n - a[i]][idx[n - a[i]].size() - 1]<<" ";
        idx[n - a[i]].pop_back(); // 按顺序输出帽子号码
    }
    cout<<endl;
    return 0;
}
```