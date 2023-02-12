---
title: LeetCode-481 神奇字符串 
date: 2022-10-31 14:25:00
updated:
tags: [计算机, 算法, LeetCode, C++, 字符串]
categories: LeetCode题解
cover: 
description: LeetCode-481「神奇字符串」的思考与解答
---
### [LeetCode-481 神奇字符串](https://leetcode.cn/problems/magical-string/)

### Solution 1
这个数列的构造方式有点特殊, 其本质上是一种 "自我指涉" . 只需要几句话就可以生成这个数列:
- 数列由连续的 $1$ 和 $2$ 交错构成
- 数列第一项为 $1$ 
- 数列的第 $i$ 项等于数列第 $i$ 个连续段的长度

其中相对难理解的就是第三点. 直接考虑整个数列的特殊性质是如何被满足的有点困难, 我们从第一项开始构造整个数列.
记这个数列为 $\{a_n\}$ . 因为 $a_1 = 1$ , 所以第一段, 也就是连续的 $1$ 的个数为 $1$ , 这意味着 $a_2 = 2$ , 这又告诉我们第二段 (也就是连续的 $2$ ) 长度为 $2$, 所以 $a_3 = 2$ ... 依此类推. 构造时, 数列的第 $i$ 项先生成, 之后我们不断根据已有的信息慢慢扩展整个数列, 这让人很自然地联想到双指针. 

代码如下:
```C++
class Solution {
public:
    int magicalString(int n) {
        string s = "01";
        int ans = 0;
        int p = 1;
        int i = 1;
        char v = '1';
        while (p <= n) {
            if (s[i] == '2') {
                s.push_back(v);
            }
            v = '1' + '2' - v;
            s.push_back(v);
            i++;
            p++;
        }
        for (int i = 1; i <= n; i++) {
            ans += s[i] == '1';
        }
        return ans;
    }
};
```