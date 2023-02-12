---
title: LeetCode-517 超级洗衣机 
date: 2022-09-12 00:38:00
updated:
tags: [计算机, 算法, LeetCode, C++, 贪心]
categories: LeetCode题解
cover: https://raw.githubusercontent.com/wsylab/wsyPicGo/main/img/photo-1648627667032-d02d79b28066.avif
description: LeetCode-517「超级洗衣机」的思考与解答
---
### [LeetCode-517 超级洗衣机](https://leetcode.cn/problems/super-washing-machines/)

### Solution 1
记平均衣服数为 $avg$ . 考虑洗衣机的 "流量" . 对于每台洗衣机, 因为一次操作中至多减少一件衣服, 而可能加上多件衣服, 所以我们考虑衣服过多的洗衣机的流量, 至少是 $machines[i] - avg$ . 与此同时, 对于左侧的洗衣机, 如果多出来了一定要流向右侧, 因此我们还要统计左右洗衣机组的衣服差. $ans$ 由两者间的最大值决定.
贪心算法正确性的证明见 [【刷穿 LeetCode】517. 超级洗衣机 : 详解为何能取到理论最小操作次数](https://blog.51cto.com/acoier/5318550) .
代码如下:
```C++
class Solution {
public:
    int findMinMoves(vector<int>& machines) {
        int n = machines.size();
        int sum = 0;
        for (int v: machines) {
            sum += v;
        }
        if (sum % n != 0) {
            return -1;
        }
        int ans = 0;
        int res = 0;
        for (int i = 0; i < n; i++) {
            machines[i] -=  sum / n;
            res += machines[i];
            ans = max(ans, max(machines[i], abs(res)));
        }
        return ans;
    }
};
```