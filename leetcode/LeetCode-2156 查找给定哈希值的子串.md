---
title: LeetCode-2156 查找给定哈希值的子串 
date: 2022-05-17 10:06:00
updated:
tags: [计算机, 算法, LeetCode, C++, 滑动窗口]
categories: LeetCode题解
cover: https://raw.githubusercontent.com/wsylab/wsyPicGo/main/img/photo-1421789665209-c9b2a435e3dc
description: LeetCode-2156「查找给定哈希值的子串」的思考与解答
---
### [LeetCode-2156 查找给定哈希值的子串](https://leetcode.cn/problems/find-substring-with-given-hash-value/)

### Solution 1
构思非常好的一道题.
需要求出长度为 $k$ 字串的哈希值, 并选择第一个哈希值等于目标的字串, "第一个"很容易让人联想到从左向右维护一个长度为 $k$ 的窗口. 考虑窗口滑动的哈希值变化, 会发现这道题的棘手之处: 从左向右移动时, 哈希值需要除以 $p$ 再取模, 然而我们只能维护取模后的哈希值, 而 $p$ 与 $m$ 不一定互素, 即我们不一定能求得 $p$ 在模 $m$ 意义下的逆元, 因此这一想法很难实施. 这时不妨逆向思考一下: 除法难于进行, 有没有办法转化成乘法? 当然有! 从右向左滑动窗口即可. 想到这一点后面的代码就不难实现了.
这道题由于数据大小的原因需要频繁取模, 需要注意一下.
代码如下: 
```C++
class Solution {
public:
    string subStrHash(string s, int power, int modulo, int k, int hashValue) {
        // 只需要考虑power % modulo即可
        power %= modulo;
        // 记录最靠左的合法字串头坐标
        int pos = 0;
        // 当前窗口字串哈希值
        long long now = 0;
        int n = s.size();
        // 预处理power^i的值
        vector<long long> expmod(k, 1);
        for (int i = 1; i < k; i++) {
            expmod[i] = expmod[i - 1] * power;
            expmod[i] %= modulo;
        }
        // 最右侧子串
        for (int i = n - 1; i >= n - k; i--) {
            now += val(s[i]) * expmod[k + i - n];
            now = now % modulo;
        }
        if (now == hashValue) {
            pos = n - k;
        }
        for (int i = n - 2; i >=k - 1; i--) {
            now -= val(s[i + 1]) * expmod[k - 1];
            now %= modulo;
            now *= power;
            now += val(s[i - k + 1]);
            now %= modulo;
            // now取模后可能为负值, 加上modulo即可
            if(now < 0) {
                now += modulo;
            }
            // 注意找到一个子串后不能立刻返回, 我们需要的是最左侧的
            if (now == hashValue) {
                pos = i- k + 1;
            }
        }
        // 得出答案
        return s.substr(pos, k);
    }

    int val(char c) {
        return c - 'a' + 1;
    }
};
```