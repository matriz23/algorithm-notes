---
title: CodeForces-1196D2 RGB Substring (hard version) 
date: 2022-07-21 15:13:00
updated:
tags: [计算机, 算法, CodeForces, C++, 字符串, 前缀和]
categories: CodeForces题解
cover: https://raw.githubusercontent.com/wsylab/wsyPicGo/main/img/photo-1597279393696-d5701aee7bf7
description: CodeForces-1196D2「RGB Substring (hard version)」的思考与解答
---
### [CodeForces-1196D2 RGB Substring (hard version)](https://codeforces.com/problemset/problem/1196/D2)
### 题目大意 
给定 $n, k$ 和一个由 $R, G, B$ 组成的字符串 $s$ ,
现在需要修改字符串 $s$ 使得存在一个长度 $\geq k$ 的子串是字符串 $RGBRGBRGB...$ 的子串. 求最少的修改次数.
### Solution 1
由于 $RGBRGBRGB...$ 的循环节长度为 $3$ , 我们分别考虑 $3$ 种匹配模式. 对于每一种, 把 $s$ 与无限长的串进行匹配, 如果 $s[i]$ 不符合就记录 $val[i] = 1$ . 寻找长度为 $k$ 的最小区间和, 可以用滑动窗口, 也可以用前缀和处理.
代码如下:
```C++
#include <bits/stdc++.h>
using namespace std;
int main() {
    int q;
    cin>>q;
    while (q--) {
        int n, k;
        cin>>n>>k;
        string s;
        cin>>s;
        string t[3] = {"RGB", "GBR", "BRG"};
        vector<vector<int>> val(3, vector<int>(n, 0));
        for (int i = 0; i < 3; i++) {
            for (int j = 0; j < n; j++) {
                val[i][j] = (j == 0)? (s[j] != t[i][j % 3]): val[i][j - 1] + (s[j] != t[i][j % 3]);
            }
        }
        int ans = INT_MAX;
        for (int i = 0; i < 3; i++) {
            ans = min(ans, val[i][k - 1]);
            for (int j = k; j < n; j++) {
                ans = min(ans, val[i][j] - val[i][j - k]);
            }
        }
        cout<<ans<<endl;
    }
    return 0;
}
```

> 这道题比较重要的地方就是匹配的思路. 从大的尺度来看问题, 把用无限长的串与原串进行匹配, 再寻找子串的匹配数. 如果一开始考虑太多子串的细节, 反而比较难做.