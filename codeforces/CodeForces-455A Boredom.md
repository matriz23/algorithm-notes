---
title: CodeForces-455A Boredom 
date: 2022-06-22 15:42:00
updated:
tags: [计算机, 算法, CodeForces, C++]
categories: CodeForces题解
cover: https://raw.githubusercontent.com/wsylab/wsyPicGo/main/img/photo-1516469635987-fcdf02d6017c
description: CodeForces-455A「Boredom」的思考与解答
---
### [CodeForces-455A Boredom](https://codeforces.com/problemset/problem/455/A)

### 题目大意
输入正整数 $n$ 与长度为 $n$ 的数组 $a[n]$ , 可以执行如下操作: 从数组中删去 $a[i]$ , 同时删去所有等于 $a[i] + 1$ 或 $a[i] - 1$ 的数, 得到 $a[i]$ 分. 返回多次操作后得分的最大值.

### Solution 1
这道题与经典动态规划问题[LeetCode-198 打家劫舍](https://leetcode.cn/problems/house-robber/)很相似. 注意到删除 $a[i]$ 仅会删除其自身, 而不会影响与其相等的元素, 故我们必然可以得到这些分数, 即值为 $k$ 的元素出现了 $cnt[k]$ 次, 在 $k$ 上我们至多可以得到 $k × cnt[k]$ 分, 把 $k × cnt[k]$ 看成 $k$ 的价值, 记 $nums[i]$ 为第 $i$ 个值(不妨把值按从小到大的顺序排列), $scores[i] = nums[i] × cnt[nums[i]]$ 为第 $i$ 个元素提供的得分, $dp[i]$ 为前 $i$ 个值能够取得的最大得分, 则有如下状态转移方程:
$$
dp[i] = 
\begin{cases}
dp[i - 1] + scores[i],\ if\ nums[i] \not =nums[i - 1] + 1\\
max(dp[i - 1], dp[i - 2] + scores[i]),\ if\ nums[i] =nums[i- 1] + 1
\end{cases}
$$
假设共有 $m$ 个互异的元素, 所求最大得分即为 $dp[m - 1]$.
代码如下:
```C++
#include <bits/stdc++.h>
using namespace std;
int main() {
    int n;
    cin>>n;
    int temp = 0;
    map<int, long long> nummap; // 记录值k出现的次数
    for (int i = 0; i < n; i++) {
        cin>>temp;
        nummap[temp]++;
    }
    vector<vector<long long>> scores; // 存储值k提供的得分
    auto it = nummap.begin();
    while (it != nummap.end()) {
        vector<long long> tmpvec;
        tmpvec.push_back(it->first);
        tmpvec.push_back(it->first * it->second);
        scores.push_back(tmpvec);
        it++;
    }
    int m = scores.size();
    vector<long long> dp(m, 0);
    dp[0] = scores[0][1];
    if (m == 1) { // m == 1特判
        cout<<dp[0]<<endl;
        return 0;
    }
    dp[1] = (scores[1][0] == scores[0][0] + 1) ? max(dp[0], scores[1][1]): dp[0] + scores[1][1]; // dp[1] 单独处理
    for (int i = 2; i < m; i++) {
        dp[i] = (scores[i][0] == scores[i - 1][0] + 1) ? max(dp[i - 2] + scores[i][1], dp[i - 1]): dp[i - 1] + scores[i][1];
    }
    cout<<dp[m - 1]<<endl;
    return 0;
}
```

### Solution 2
这道题计数/DP用桶排序更简单. 桶排序下DP不需要分类讨论, 另外一点可以优化的就是 $scores$ 数组本身就可以充当 $dp$ 数组.
代码如下:
```C++
#include <bits/stdc++.h>
using namespace std;
int main() {
    int n;
    cin>>n;
    vector<long long> scores(100001, 0);
    int temp = 0;
    for (int i = 0; i < n; i++) {
        cin>>temp;
        scores[temp] += temp;
    }
    for (int i = 2; i <= 100000; i++) {
        scores[i] = max(scores[i - 2] + scores[i], scores[i - 1]);
    }
    cout<<scores[100000]<<endl;
    return 0;
}
```