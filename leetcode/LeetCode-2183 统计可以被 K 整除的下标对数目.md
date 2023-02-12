---
title: LeetCode-2183 统计可以被 K 整除的下标对数目
date: 2022-05-09 15:29:00
updated:
tags: [计算机, 算法, LeetCode, C++, 数学, 计数]
categories: LeetCode题解
cover: https://raw.githubusercontent.com/wsylab/wsyPicGo/main/img/photo-1509180756080-d110bee1e772
description: LeetCode-2183「统计可以被 K 整除的下标对数目」的思考与解答
---
### [LeetCode-2183 统计可以被 K 整除的下标对数目](https://leetcode.cn/problems/count-array-pairs-divisible-by-k/)

### Solution 1
计数问题, 思路要清晰.
我们思考怎样的 $x,y$ 满足乘积是 $k$ 的倍数? 或者说, 对于一个给定的 $x$ , 什么样的 $y$ 能够保证两数之积为 $k$ 的倍数?
我们只管心共同因子的部分, $gcd(x, k)$ 是 $k$ 中 $x$ 所提供的部分, 从这方面考虑, 我们需要 $y$ 来提供 $k / gcd(x, k)$ 的那部分, 因此应当有 $y = m * k / gcd(x, k)$. 
怎样进行相对快速的统计呢? 对每个 $k / gcd(x, k)$, 我们需要知道其倍数在数组中出现了多少次, 因此我们先统计数组中各个数的分布情况, 再来统计每个数的倍数在数组中出现了多少次. 
需要注意的是, 这种统计方法会把 $(i, j)(i \neq j)$ 计算两次, 把 $(i, i)$ 计算一次. 对于初步计算出的答案, 我们需要统计后一种情况的个数, 减去后再除以二得到最终结果.
代码如下: 
```C++
class Solution {
public:
    long long countPairs(vector<int>& nums, int k) {
        int maxN = k;
        for (int num: nums) {
            maxN = max(maxN, num);
        }
        vector<int> cnt(maxN + 1, 0);
        for (int num: nums) {
            cnt[num]++;
        }
        for (int i = 1; i <= maxN; i++) {
            for (int j = 2 * i; j <= maxN; j += i) {
                cnt[i] += cnt[j];
            }
        }
        long long ans = 0;
        for (int num: nums) {
            ans += cnt[k / __gcd(num, k)];
            if ((long long)(num) * (long long)(num) % k == 0) {
                ans--;
            }
        }
        return ans / 2;
    }
};
```