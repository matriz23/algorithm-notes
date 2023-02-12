---
title: CodeForces-510C Fox And Names 
date: 2022-07-21 14:36:00
updated:
tags: [计算机, 算法, CodeForces, C++, 拓扑排序, 图论, 字典序, 字符串]
categories: CodeForces题解
cover: https://raw.githubusercontent.com/wsylab/wsyPicGo/main/img/photo-1619017236031-6f55d516f49a
description: CodeForces-510C「Fox And Names」的思考与解答
---
### [CodeForces-510C Fox And Names](https://codeforces.com/problemset/problem/510/C)
### 题目大意
给定 $n$ 个字符串, 如果存在一种新的字典序符合这些字符串的排列方式, 则输出一种字典序; 否则输出 $Impossible$ .
### Solution 1
如果 $s_i$ 和 $s_{i + 1}$ 不存在前缀关系, 则我们可以找到一对字母的排序关系. 我们的目标就是通过一对对字母的关系重构出所有字母的排序关系. 这是一个典型的拓扑排序问题. 先根据字母对建图, 再利用拓扑排序判断是否有环.
代码如下:
```C++
#include <bits/stdc++.h>
using namespace std;
int main() {
    int n;
    cin>>n;
    vector<string> s(n, "");
    vector<vector<int>> mp(26); // mp[] 记录有向边指向的点
    vector<int> in(26); // in 记录入度
    for (int i = 0; i < n; i++) {
        cin>>s[i];
    }
    // 建立有向图
    for (int i = 0; i < n - 1; i++) {
        int pos = 0;
        int len = min(s[i].size(), s[i + 1].size());
        while (s[i][pos] == s[i + 1][pos] && pos < len) {
            pos++; // 找出第一个相异的字符位置 & 判断前缀关系
        }
        if (pos < len) { // 因为相异跳出循环, 获得一对字母的关系
            mp[s[i][pos] - 'a'].push_back(s[i + 1][pos] - 'a'); // 更新边
            in[s[i + 1][pos] - 'a']++; // 更新入度
        }
        else { // 因为字符串长度限制跳出循环, 说明存在前缀关系
            if (s[i].size() > s[i + 1].size()) { // 如果前缀排在后面, 不符合字典序排序
                cout<<"Impossible"<<endl;
                return 0;
            }
        }
    }
    // 拓扑排序
    queue<int> q; // 队列存储入度为0的点
    for (int i = 0; i < 26; i++) {
        if (in[i] == 0) {
            q.push(i);
        }
    }
    string ans = ""; // 列表存储顶点
    while (!q.empty()) {
        int temp = q.front();
        q.pop();
        ans += (char)(temp + 'a');
        for (int i = 0; i < mp[temp].size(); i++) { 
            in[mp[temp][i]]--; // 删除边(这一步不需要模拟), 同时更新入度 
            if (in[mp[temp][i]] == 0) {
                q.push(mp[temp][i]); // 更新q
            }
        }
    }
    if (ans.size() == 26) { // 如果所有顶点都加入了, 说明不剩下边了, 能够拓扑排序
        cout<<ans<<endl;
    }
    else { // 有向图存在环, 无法重构字典序
        cout<<"Impossible"<<endl;
    }
    return 0;
}
```