---
title: LeetCode-2311 小于等于 K 的最长二进制子序列 
date: 2022-06-22 10:46:00
updated:
tags: [计算机, 算法, LeetCode, C++, 贪心]
categories: LeetCode题解
cover: https://raw.githubusercontent.com/wsylab/wsyPicGo/main/img/photo-1504718534688-33202e2a194d
description: LeetCode-2311「小于等于 K 的最长二进制子序列」的思考与解答
---
### [LeetCode-2311 小于等于 K 的最长二进制子序列](https://leetcode.cn/problems/longest-binary-subsequence-less-than-or-equal-to-k/)

### Solution 1
本题要求的子序列可以是不连续的, 同时允许前导 $0$ 的存在, 一个重要的性质就是对于某个合法子序列, 我们加上它前面的所有 $0$ 构成一个更优的合法子序列. 现在考虑怎么获得这个合法子序列, 首先由于前导 $0$ 的存在, 我们倾向于让这个子序列更加靠后, 这样前面的 $0$ 也会变多; 事实上, 这种想法是正确的.
在子序列的寻找过程中, $01$ 总是比 $10$ 好, 所以选择 $1$ 时也是右侧的 $1$ 更好. 我们考虑这样一种贪心的寻找方法: 从最右侧开始, 不断向左增加子序列的长度, 直到子序列超过 $k$ 为止. 对于这个子序列, 加上所有先导 $0$ 即为所求最长合法子序列.
代码如下:
```C++
class Solution {
public:
    int longestSubsequence(string s, int k) {
        int ans = 0;
        int pow = 1; // 记录当前位上2的幂次
        int n = s.size();
        for (int i = n - 1; i >= 0; i--) { 
            if (s[i] == '0' || pow <= k) { // 如果是0, 就选上, 如果是1, 那么不超过当前k也选上
                k -= pow * (s[i] - '0'); // 更新k, 减去相当于求和, 不过这样能够避免long long问题, 在0过多时计算pow会出错
                ans++;
            }
            if (pow <= k) { //更新幂次
                pow *= 2;
            }
        }
        return ans;
    }
};
```