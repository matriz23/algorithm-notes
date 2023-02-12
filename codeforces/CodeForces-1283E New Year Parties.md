---
title: CodeForces-1283E New Year Parties 
date: 2022-07-18 12:13:00
updated:
tags: [计算机, 算法, CodeForces, C++, 贪心]
categories: CodeForces题解
cover: https://raw.githubusercontent.com/wsylab/wsyPicGo/main/img/photo-1514876246314-d9a231ea21db
description: CodeForces-1283E「New Year Parties」的思考与解答
---
### [CodeForces-1283E New Year Parties](https://codeforces.com/problemset/problem/1283/E)
### 题目大意
给定一个数组 $a$ , 对于每个元素 $a_i$ , 可以选择将其替换为 $a_i - 1, a_i, a_i + 1$ 中的一个. 返回修改后数组不同的元素个数的最小值和最大值.
### Solution 1
先讨论最小值. 我们需要尽可能地将不同的值合并, 合并时向左还是向右其实没有区别. 对于一个元素 $x$ , 如果 $x - 1$ 已经有了, 我们就把它和 $x - 1$ 合并, 否则把 $x$ 替换成 $x + 1$ , 这样会增大与后续 $x + 1$ 和 $x + 2$ 合并的可能性. 一种形式上更优雅的统计方式是, 记 $cnt[i]$ 为 值为 $i$ 的元素的数量, 我们按 $i$ 逐个统计 $cnt[i]$ , 遇到 $cnt[i]$ , 计数 $+ 1$ , 之后从 $i + 3$ 开始统计. 这是因为 $i + 2$ 与 $i + 1$ 是能与 $i$ 合并的.
求最小值的代码如下:
```C++
for (int i = 1; i <= n;) {
    if (cnt[i]) {
        ans++;
        i += 3;
    }
    else {
        i += 1;
    }
}
```
再讨论最大值. 最大值希望尽可能分开元素, 我们同样试试贪心算法. 对如果 $cnt[i] \geq 2$ , 我们可以不断向一侧延展直至遇到空位(这一过程可以想象成水中的涟漪) . 我们优先向左侧延展, 如果还剩下值, 先算上自身的, 如果还有剩余, 再考虑右侧( $i + 1$ ). 如果右侧是空位, 我们可以把右侧填上, 再从 $i + 2$ 继续. 如果右侧不是空位, 我们同样分配给它一个, 再从 $i + 1$ 继续. 尽管一个数只能移动一次的限制让这部分贪心算法可能会让人混淆, 但仔细考虑之后会发现, 我们的移动策略实际上已经保证了不会让一个数连续移动两次). 如果追求更清晰的写法, 可以另开一个数组做相关记录.
求最大值的代码如下:
```C++
for (int i = 1; i <= n; i++) {
    if (cnt[i] == 0) {
        continue; // 没有数就跳过
    }
    if (cnt[i - 1] == 0) {
        cnt[i]--; // 左侧有空缺, 优先填左侧的
        ans++;
    }
    if (cnt[i]) {
        ans++; // 如果填完后自己还有剩余, 加上自己
    }
    if (cnt[i] >= 2) { // 如果自己的数量还很多, 考虑右侧
        if (cnt[i + 1] == 0) { // 右侧有空缺, 我们把右侧填上, 再跳过右侧
            ans++;
            i++;
        } else if (cnt[i + 1] > 0) { // 右侧没有空缺, 借给右侧一个, 便于右侧进一步传递至最右侧的空位
            cnt[i]--;
            cnt[i + 1]++;
        }
    }
}
```
两部分合并之后, 最终的代码如下:
```C++
#include <bits/stdc++.h>
using namespace std;
const int MAXN=2e5+10;
int main() {
    int n;
    cin>>n;
    vector<int> cnt(MAXN, 0);
    int ans = 0;
    for (int i = 0; i < n; i++) {
        int temp;
        cin>>temp;
        cnt[temp]++;
    }
    for (int i = 1; i <= n;) {
        if (cnt[i]) {
            ans++;
            i += 3;
        }
        else {
            i += 1;
        }
    }
    cout<<ans<<" ";
    ans = 0;
    vector<int> a(MAXN, 0);
    for (int i = 1; i <= n; i++) {
        if (cnt[i] == 0) {
            continue;
        }
        if (cnt[i - 1] == 0) {
            cnt[i]--;
            ans++;
        }
        if (cnt[i]) {
            ans++;
        }
        if (cnt[i] >= 2) {
            cnt[i]--;
            cnt[i + 1]++;
            if (cnt[i + 1] == 0) {
                ans++;
                i++;
            }
        }
    }
    cout<<ans<<endl;
    return 0;
}
```