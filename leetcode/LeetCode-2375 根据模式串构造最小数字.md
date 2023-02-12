---
title: LeetCode-2375 根据模式串构造最小数字 
date: 2022-08-19 08:15:00
updated:
tags: [计算机, 算法, LeetCode, C++, 构造, 贪心]
categories: LeetCode题解
cover: https://raw.githubusercontent.com/wsylab/wsyPicGo/main/img/photo-1507720708252-1ddeb1dbff34
description: LeetCode-2375「根据模式串构造最小数字」的思考与解答
---
### [LeetCode-2375 根据模式串构造最小数字](https://leetcode.cn/problems/construct-smallest-number-from-di-string/)

### Solution 1
根据长度为 $n$ 的 $pattern$ 字符串构造长度为 $n + 1$ 的 $ans$ , 由于每个数字字符只能用 $1$ 次, 而且要求 $ans$ 字典序尽可能小, 我们考虑贪心的构造模式. 遇到 $I$ , 优先选择当前剩下的最小的字符; 遇到 $D$ , 观察连续有多少个 $D$ 相邻, 从后向前逆序构造, 每次优先选择当前剩下的最小的字符. 这一部分的实现也可以用反转 $123456789$ 这个字符串的部分区间实现.
代码如下:
```C++
class Solution {
public:
    string smallestNumber(string pattern) {
        int n = pattern.size();
        pattern += 'I'; // 后面补一个 $I$ , 方便统一处理
        char p = '1'; // p 代表当前可用的最小字符
        string ans = "";
        set<char> book; // 记录用过的数字
        for (int i = 0; i < n + 1; i++) {
            if (pattern[i] == 'I') { // 遇到 I , 优先选最小字符
                ans += p;
                book.insert(p); // 记录
                p++;
            }
            else { // 遇到 D , 记录有连续多少个 D , 再构造.
                int cnt = 0;
                for (int j = i; j < n; j++) {
                    cnt++;
                    if (pattern[j + 1] == 'I') {
                        cout<<cnt<<endl;
                        ans += p + cnt;
                        book.insert(p + cnt);
                        break;
                    }
                }
            }
            while (book.count(p)) { // 更新 p
                p++;
            }
        }
        return ans;
    }
};
```