---
title: LeetCode-2178 拆分成最多数目的正偶数之和 
date: 2022-05-09 19:18:00
updated:
tags: [计算机, 算法, LeetCode, C++, 二分, 构造, 贪心]
categories: LeetCode题解
cover: https://raw.githubusercontent.com/wsylab/wsyPicGo/main/img/photo-1488543882437-49f6f714ad05
description: LeetCode-2178「拆分成最多数目的正偶数之和」的思考与解答
---
### [LeetCode-2178 拆分成最多数目的正偶数之和](https://leetcode.cn/problems/maximum-split-of-positive-even-integers/)

### Solution 1
题目要求把一个数拆分成互不相等的偶数之和, 并且要求给出一个元素个数最多的构造.
首先, 奇数必定不存在这样的拆分. 考虑偶数的情况, 自己试验几个数之后很容易发现一个贪心的构造方法(证明不难): 把$N$拆分成 $2, 4,...,2
k$ 与一个**无法再分**的 $M$ .接下来考虑约束关系. 因为 $M$ 不能继续分割, 可以得到 $2k + 2 \leq M < (2k + 2) + (2k + 4) = 4k + 6$ , 再由 $M$ 是偶数可以得到 $2k + 2\leq M \leq 4k + 4$, 再由和的关系, 有 $k^2 + 5
k + 4 \geq N \geq k^2 + 3k + 2$ . 需要求出满足这一式子的最大 $k$ . 事实上, 对于一个给定的 $N$, 满足这一式子的 $k$ 只有一个. 我们可以从两个角度证明这个结论: 一方面, 我们可以考虑如果 $k+1$ 也满足这个式子, 那么把数列 $2,4,...,2k,2k+2$ 最后一项 $2k + 2$加到 $M'$ 上, 就所得的等于 $k$ 构造中的 $M$ , 这与 $M$ 不可分矛盾. 另一方面, 我们可以证明区间 $[ k^2 + 3k + 2, k^2 + 5
k + 4]$ 两两不重合, 最多有一个区间框住 $N$. 综上求出一个 $k$ 即可. 这里我们考虑 $k^2 + 5
k + 4 \geq N$ , 求解其左边界, 由单调性质可以考虑使用二分搜索.
实战中有一些细节需要注意:
- 由于$long\ long$ 相乘会越界, 判断 $k^2 + 5
k + 4 \geq N$ 时需要作除法, 这时候需要考虑整型除法去尾的性质, 分 $N \equiv 0 \ mod\ k$ 与 $N \not\equiv 0 \ mod\ k$ 讨论.
- 由于使用了除法, 二分搜索时对 $mid$ 是否为 $0$ 需要讨论, 我这里直接把 $N$ 较小时 $mid$ 可能为 $0$ 的情况单独讨论了.
- 结果返回 $long\ long$ 的时候, 涉及结果运算的地方全部要使用 $long\ long$, 不然很可能越界.
代码如下:
```C++
class Solution {
public:
    vector<long long> maximumEvenSplit(long long finalSum) {
        vector<long long>ans;
        if (finalSum % 2 == 1) {
            return ans;
        }
        // mid可能为0
        if (finalSum == 2) {
            ans.push_back(2);
            return ans;
        }
        // mid可能为0
        if (finalSum == 4) {
            ans.push_back(4);
            return ans;
        }
        // 记得开long long
        long long left = 1;
        long long right = finalSum - 5;
        // 二分搜索左边界
        while (left < right) {
            long long mid = left + (right - left) / 2;
            if (isOver(mid, finalSum)) {
                right = mid;
            }
            else {
                left = mid + 1;
            }
        }
        for (long long i = 1; i <= left; i++) {
            ans.push_back(2 * i);
            finalSum -= 2 * i;
        }
        ans.push_back(finalSum);
        return ans;
    }
    // 记得开long long
    bool isOver(long long mid, long long finalSum) {
        if ((finalSum - 4) % mid == 0) {
            return (mid + 5) >= (finalSum - 4) / mid;
        }
        else {
            return (mid + 4) >= (finalSum - 4) / mid;
        }
    }
};
```

### Solution 2
上面是我自己的解法, 构造和论证有点啰嗦, 以下是更为简洁的[官方做法](https://leetcode.cn/problems/maximum-split-of-positive-even-integers/solution/chai-fen-cheng-zui-duo-shu-mu-de-ou-zhen-dntf/): 
我们考虑一种构造方式: 从 $N$ 上逐步拆出当前最小的正偶数, 直到剩余数值不大于当前被拆分的最大偶数, 这时把剩余数值加到这个最大的偶数上.
假设我们利用这种拆分方式把 $N$ 拆分成了 $k$ 个正偶数, 则可以计算出 $N$ 的上下界: $k^2 + k = \sum_{i = 1}^k 2i \leq N \leq \sum_{i = 1}^{k} 2i + 2k = k^2 + 3k$, 现在假设 $N$ 能够被拆分成 $k + 1$ 个正整数, 则必定有 $N \geq \sum_{i = 1}^{k + 1}2i = k^2 + 3k + 2$, 这与前面推导出的结论矛盾, 故 $k$ 就是所能拆分 出的最大个数.
代码如下:
```C++
class Solution {
public:
    vector<long long> maximumEvenSplit(long long finalSum) {
        if (finalSum % 2) {
            return {};
        }
        vector<long long> res = {2};
        finalSum -= 2;
        while (res.back() < finalSum) {
            finalSum -= res.back() + 2;
            res.push_back(res.back() + 2);
        }
        res.back() += finalSum;
        return res;
    }
};
```
不得不感叹一句, 与我的解答相比, 官方的思路更清晰, 构造更加直白. 下次用贪心写题目时, 还是从贪心的角度出发比较好, 能少绕就少绕.