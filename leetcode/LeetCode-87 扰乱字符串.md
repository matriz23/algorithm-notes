---
title: LeetCode-87 扰乱字符串 
date: 2022-09-19 16:15:00
updated:
tags: [计算机, 算法, LeetCode, C++, 动态规划, 字符串]
categories: LeetCode题解
cover: https://raw.githubusercontent.com/wsylab/wsyPicGo/main/img/photo-1647876012576-ab84224b7a74.avif
description: LeetCode-87「扰乱字符串」的思考与解答
---
### [LeetCode-87 扰乱字符串](https://leetcode.cn/problems/scramble-string/)

### Solution 1
"扰乱字符串" 的操作逻辑是, 分割区间, 再选择交换还是不交换. 注意到对一个大区间进行操作后, 两个子区间的元素不会再有交集, 也就是说, 问题变成了更小的子问题. 定义 $f(i_1, j_1, i_2, j_2)$ 为 $s_1[i_1: j_1]$ 是否为 $s_2[i_2: j_2]$ 的扰乱字符串. 我们来考虑是否进行区间交换, 枚举可能的分割点转化为更小的子问题. 
注意到两个字符串互为扰乱字符串的一些基本性质: 长度相同, 字符集合相同, 可以进行一些剪枝. 另外, 由于区间长度的限制, 我们 $f$ 函数的四个维度实际上只需要三个即可.
代码如下:
```C++
class Solution {
public:
    int n;
    string _s1, _s2;
    int dp[31][31][31];
    bool f(int i1, int j1, int i2, int j2) {
        // 代表 s1[i1: j1] 和 s2[i2: j2] 是否匹配
        // 边界情况: 长度只有 1
        if (i1 == j1) {
            return _s1[i1] == _s2[i2];
        }
        // 查看备忘录
        if (dp[i1][j1][i2] != -1) {
            return dp[i1][j1][i2];
        }
        // 区间总长度为 j1 - i1 + 1
        // 考虑不交换
        set<char> l_1, l_2;
        for (int len = 1; len <= j1 - i1; len++) {
            l_1.insert(_s1[i1 + len - 1]);
            l_2.insert(_s2[i2 + len - 1]);
            if (l_1 == l_2) {
                if (f(i1, i1 + len - 1, i2, i2 + len - 1) && f(i1 + len, j1, i2 + len, j2)) {
                    dp[i1][j1][i2] = 1;
                    return true;
                }
            }
        }
        // 考虑交换
        l_1.clear();
        set<char> r_2;
        for (int len = 1; len <= j1 - i1; len++) {
            l_1.insert(_s1[i1 + len - 1]);
            r_2.insert(_s2[j2 - len + 1]);
            if (l_1 == r_2) {
                
                if (f(i1, i1 + len - 1, j2 - len + 1, j2) && f(i1 + len, j1, i2, j2 - len)) {
                    dp[i1][j1][i2] = 1;
                    return true;
                }
            }
        }
        dp[i1][j1][i2] = 0;
        return false;
    }
    bool isScramble(string s1, string s2) {
        for (int i = 0; i < 31; i++) {
            for (int j = 0; j < 31; j++) {
                for (int k = 0; k < 31; k++) {
                    dp[i][j][k] = -1;
                }
            }
        }
        n = s1.size();
        _s1 = s1;
        _s2 = s2;
        return f(0, n - 1, 0, n - 1);
    }
};

```