---
title: CodeForces-1042C Array Product 
date: 2022-07-15 10:44:00
updated:
tags: [计算机, 算法, CodeForces, C++, 构造, 贪心]
categories: CodeForces题解
cover: https://raw.githubusercontent.com/wsylab/wsyPicGo/main/img/photo-1475737154378-501d2de7fd94
description: CodeForces-1042C「Array Product」的思考与解答
---
### [CodeForces-1042C Array Product](https://codeforces.com/problemset/problem/1042/C)
### 题目大意
给你一个长度为$n$的整数序列, 你可以执行以下两种操作:
- 选 $0<i,j<=n,i\not =j$ , 将 $a_j$ 替换为 $a_i × a_j$ , 删除 $a_i$ .
- 选一个未被删除的 $a_i$ , 将其删除. **该操作在任意时刻均可执行，最多执行一次**.
你需要操作 $n - 1$ 次, 使得剩下的数最大. 由于这个数可能非常大, 输出你的操作序列.
### Solution 1
题目不难, 但是一些特殊情况的非常需要细致的思考. 
操作的核心在于删除某时刻的一个 $a_i$ , 实际上相当于选择一部分元素删除, 使得剩下的和最大. 如果有负数, 我们希望负数个数为偶数. 如果有 $0$ 我们也希望尽可能将 $0$ 全部删掉; 需要注意的是不能把所有元素都删了, 有时候剩下一个 $0$ 已经是最优解了.
代码如下:
```C++
#include <bits/stdc++.h>
using namespace std;
int main() {
    int n;
    cin>>n;
    vector<int> a(n + 1, 0);
    vector<int> idx_0, idx_1, idx_2; //0 ,正, 负
    int val = -1e9 - 7;
    int idx = -1;
    for (int i = 1; i <= n; i++) {
        cin >> a[i];
        if (a[i] == 0) {
            idx_0.push_back(i);
        } else if (a[i] > 0) {
            idx_1.push_back(i);
        } else if (a[i] < 0) {
            idx_2.push_back(i);
            if (a[i] > val) {
                val = a[i];
                idx = i;
            }
        }
    }
    if (idx_0.size() == 0) {
        if (idx_2.size() % 2 == 0) {
            for (int i = 2; i <= n; i++) {
                cout<<1<<" "<<i<<" "<<1<<endl;
            }
        }
        else {
            cout<<2<<" "<<idx<<endl;
            int pos = (idx == 1)? 2: 1;
            for (int i = 1; i <= n; i++) {
                if (i != idx && i != pos) {
                    cout<<1<<" "<<i<<" "<<pos<<endl;
                }
            }
        }
    }
    else {
        if (idx_2.size() % 2 == 0) {
            vector<bool> flag(n + 1, true);
            for (int i = 1; i < idx_0.size(); i++) {
                cout<<1<<" "<<idx_0[i]<<" "<<idx_0[0]<<endl;
                flag[idx_0[i]] = false;
            }
            if (idx_1.size() == 0 && idx_2.size() == 0) {
                return 0;
            }
            cout<<2<<" "<<idx_0[0]<<endl;
            flag[idx_0[0]] = false;
            int pos = 1;
            while (!flag[pos] && pos <= n) {
                pos++;
            }
            for (int i = 1; i <= n; i++) {
                if (flag[i] && i != pos) {
                    cout<<1<<" "<<i<<" "<<pos<<endl;
                }
            }
        }
        else {
            vector<bool> flag(n + 1, true);
            for (int i = 1; i < idx_0.size(); i++) {
                cout << 1 << " " << idx_0[i] << " " << idx_0[0] << endl;
                flag[idx_0[i]] = false;
            }
            cout << 1 << " " << idx_0[0] << " " << idx << endl;
            flag[idx_0[0]] = false;
            if (idx_2.size() == 1 && idx_1.size() == 0) {
                return 0;
            }
            cout << 2 << " " << idx << endl;
            flag[idx] = false;
            int pos = 1;
            while (!flag[pos] && pos <= n) {
                pos++;
            }
            for (int i = 1; i <= n; i++) {
                if (flag[i] && i != pos) {
                    cout << 1 << " " << i << " " << pos << endl;
                }
            }
        }
    }
    return 0;
}
```

> 写得有点啰嗦, 更简洁的代码可以参考洛谷题解区.