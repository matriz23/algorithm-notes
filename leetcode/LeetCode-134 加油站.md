---
title: LeetCode-134 加油站
date: 2022-07-03 21:54:00
updated:
tags: [计算机, 算法, LeetCode, C++, 贪心]
categories: LeetCode题解
cover: https://raw.githubusercontent.com/wsylab/wsyPicGo/main/img/photo-1629241290025-6bb716261f5f
description: LeetCode-134「加油站」的思考与解答
---
### [LeetCode-134 加油站](https://leetcode.cn/problems/gas-station/)

### Solution 1
如果从 $x$ 出发到不了 $y$ , 则从 $x, x + 1, ..., y - 1$ 中任一站出发都到不了 $y$ , 因为 $x$ 出发是到达中间某一站时可能还有油, 而直接从中间出发初始油量必为 $0$ . 想明白这一点, 这道题目我们贪心地寻找下一站即可.
代码如下:
```C++
class Solution {
public:
    int canCompleteCircuit(vector<int>& gas, vector<int>& cost) {
        int n = gas.size();
        int start = 0;
        while (start < n) {
            int sum = 0;
            int cnt = 0;
            while (cnt < n) {
                int now = start + cnt;
                sum += gas[now % n] - cost[now % n];
                if (sum < 0) {
                    break;
                }
                cnt++;
            }
            if (cnt == n) {
                return start;
            }
            else {
                start += cnt + 1;
            }
        }
        return -1;
    }
};
```
### 一道很有趣的加油站问题: 
本题所给数据不保证存在合法解, 那么加油站在怎样的约束下能够保证一定存在一个合法解呢? 
有这样一道经典的加油站问题:
> 一个环形轨道上有n个加油站, 所有加油站的油量总和正好够车跑一圈. 证明: 总能找到其中一个加油站, 使得初始时油箱为空的汽车从这里出发, 能够顺利环行一圈回到起点.

这个结论告诉我们足够的油量是必要且充分的. 证明如下:
#### Proof 1
考虑环形轨道上的加油站必定存在一个加油站, 用它的油足够跑到下一个加油站. 这一点可以用反证法证明. 如果不存在这样的加油站, 对油量求和显然是不够车跑一圈的, 这与题设矛盾. 找到这个加油站之后, 把它和下一个加油站合并(这对汽车没有任何影响), 对新的加油站分布不断重复这一操作, 直到只剩下一个加油站为止, 显然这就是我们所找的加油站.

#### Proof 2
先让汽车油箱里装好足够多的油, 从任一加油站出发试跑一圈. 每到一个加油站时, 记录此时油箱里剩下的油量, 再把加油站的油全部装上. 试跑完一圈后, 检查刚才到各个加油站时所剩的油量, 从最少的那一个站点出发即可, 显然这种策略能保证全程油量 $>0$ .