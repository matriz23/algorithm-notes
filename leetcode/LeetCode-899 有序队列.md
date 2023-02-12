---
title: LeetCode-899 有序队列 
date: 2022-08-03 14:47:00
updated:
tags: [计算机, 算法, LeetCode, C++, 字符串, 思维]
categories: LeetCode题解
cover: https://raw.githubusercontent.com/wsylab/wsyPicGo/main/img/photo-1541913299-273fd84d10c4
description: LeetCode-899「有序队列」的思考与解答
---
### [LeetCode-899 有序队列](https://leetcode.cn/problems/orderly-queue/)

### Solution 1
$k = 1$ 时, 每次只能把字符串第一个元素移到最后, 本质上是在环上寻找最小字典序字符串, 可以把 $s$ 复制一份寻找长度为一半的最小字典序字符串;
$k \geq 2$ 时, 经过试验后可以猜想: 任意字符串都能经过有限次操作后变成严格升序的字符串. 由于 $k > 2$ 时的操作比 $k = 2$ 更强, 我们以 $k = 2$ 为例来证明猜想. 从环的意义上考虑, 选择第一个放到末尾相当没有操作, 选择第二个放到末尾相当于交换一对元素. 很显然, 经过有限次两两交换, 我们可以得到一个升序字符串.
代码如下:
```C++
class Solution {
public:
    string orderlyQueue(string s, int k) {
        if (k >= 2) {
            map<char, int> book;
            for (auto ch: s) {
                book[ch]++;
            }
            string ans = "";
            for (auto it = book.begin(); it != book.end(); it++) {
                for (int i = 0; i < it->second; i++) {
                    ans += it->first;
                }
            }
            return ans;
        }
        else {
            s += s;
            string ans = s.substr(0, s.size() / 2);
            for (int i = 1; i < s.size() / 2; i++) {
                if (s.substr(i, s.size() / 2) < ans) {
                    ans = s.substr(i, s.size() / 2);
                }
            }
            return ans;
        }
    }
};
```