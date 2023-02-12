---
title: LeetCode-2338 统计理想数组的数目 
date: 2022-07-12 13:30:00
updated:
tags: [计算机, 算法, LeetCode, C++, 数学]
categories: LeetCode题解
cover: https://raw.githubusercontent.com/wsylab/wsyPicGo/main/img/photo-1419064642531-e575728395f2
description: LeetCode-2338「统计理想数组的数目」的思考与解答
---
### [LeetCode-2338 统计理想数组的数目](https://leetcode.cn/problems/count-the-number-of-ideal-arrays/)

### Solution 1
该解法来自[0x3F大神的题解](https://leetcode.cn/problems/count-the-number-of-ideal-arrays/solution/shu-lun-zu-he-shu-xue-zuo-fa-by-endlessc-iouh/).
按末尾数字考虑理想数组 $a$ , 假设末尾是 $k$ 考虑数组相邻位的变化, 把 $a_0$ 视作从 $1$ 变化得到, 经过 $n$ 个位置的变化刚好获得 $k$ , 则以 $k$ 为结尾的数组个数即是把 $k$ 的因子分配到 $n$ 个位置的方法. 对于出现了 $c$ 次的因子 $p$ , 采用隔板法可以计算出次数, 按因子相乘再按末尾元素相加得到最终答案.
代码如下:
```C++
const int MOD = 1e9 + 7, MX = 1e4 + 1, MX_K = 13; // 至多 13 个质因数
vector<int> ks[MX]; // ks[x] 为 x 分解质因数后，每个质因数的个数列表
int c[MX + MX_K][MX_K + 1]; // 组合数

int init = []() { // 预处理
    for (int i = 2; i < MX; ++i) {
        int x = i;
        for (int p = 2; p * p <= x; ++p) {
            if (x % p == 0) {
                int k = 1;
                for (x /= p; x % p == 0; x /= p) ++k;
                ks[i].push_back(k); // 计算出可能末尾元素的因子个数
            }
        }
        if (x > 1) ks[i].push_back(1);
    }

    // 递推计算组合数
    c[0][0] = 1;
    for (int i = 1; i < MX + MX_K; ++i) {
        c[i][0] = 1;
        for (int j = 1; j <= min(i, MX_K); ++j)
            c[i][j] = (c[i - 1][j] + c[i - 1][j - 1]) % MOD;
    }
    return 0;
}();

class Solution {
public:
    int idealArrays(int n, int maxValue) {
        long ans = 0L;
        for (int x = 1; x <= maxValue; ++x) {
            long mul = 1L;
            for (int k: ks[x]) mul = mul * c[n + k - 1][k] % MOD;
            ans += mul;
        }
        return ans % MOD;
    }
};
```