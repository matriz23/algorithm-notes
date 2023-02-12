---
title: LeetCode-754 到达终点数字 
date: 2022-11-04 10:51:00
updated:
tags: [计算机, 算法, LeetCode, C++, 贪心]
categories: LeetCode题解
cover: 
description: LeetCode-754「到达终点数字」的思考与解答
---
### [LeetCode-754 到达终点数字](https://leetcode.cn/problems/reach-a-number/)

### Solution 1
考虑原问题的一个等价问题: $target$ 为正, 在数轴上从 $0$ 开始先向右走 $k$ 步, 再选择某些已走的步反转, 问能够到达 $target$ 的最小步数是多少. 
首先我们走过的路程至少要越过 $target$ . 其次注意到反转已走路程时对总路程的影响是减去一个偶数, 这要求越过 $target$ 的路程必须是一个偶数. 当这一条件满足时, 总能找到已走路程的组合, 使得反转后总路程为 $target$ .
代码如下:
```C++
class Solution {
public:
    int reachNumber(int target) {
        target = abs(target);
        int s = 0, n = 0;
        while (s < target || (s - target) & 1) {
            s += ++n;
        }   
        return n;
    }
};
```