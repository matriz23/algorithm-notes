---
title: LeetCode-2337 移动片段得到字符串 
date: 2022-07-12 14:16:00
updated:
tags: [计算机, 算法, LeetCode, C++, 思维, 字符串]
categories: LeetCode题解
cover: https://images.unsplash.com/photo-1498637841888-108c6b723fcb?ixid=MnwxMTI1OHwwfDF8cmFuZG9tfHx8fHx8fHx8MTY1NzYwNjQzOQ&ixlib=rb-1.2.1&q=85&w=2400
description: LeetCode-2337「移动片段得到字符串」的思考与解答
---
### [LeetCode-2337 移动片段得到字符串](https://leetcode.cn/problems/move-pieces-to-obtain-a-string/)

### Solution 1
因为 $'L'$ 只能向左侧的空地走, $'R'$ 只能向右侧的空地走, 所以 $'L'$ 和 $'R'$ 的相对顺序不会变化. 从这一点出发, 我们分别用两个指针遍历 $start$ 与 $target$ , 不断寻找下一个非 $'\_'$ 的字符, 如果出现字符不匹配, 显然不能满足题意; 如果字符匹配, 我们还要看一下相对位置对不对, 因为我们只能从 $start$ 向 $target$ 移动.
代码如下:
```C++
class Solution {
public:
    bool canChange(string s, string t) {
        int n = s.size();
        int p = 0;
        int q = 0;
        while (p < n && q < n) {
            while (s[p] == '_' && p < n) {
                p++;
            }
            while (t[q] == '_' && q < n) {
                q++;
            }
            if (s[p] != t[q]) {
                return false;
            }
            else {
                if (s[p] == 'L') {
                    if (p < q) {
                        return false;
                    }
                }
                else {
                    if (p > q) {
                        return false;
                    }
                }
            }
            p++;
            q++;
        }
        return true;
    }
};
```