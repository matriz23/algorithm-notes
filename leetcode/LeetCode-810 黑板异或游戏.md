---
title: LeetCode-810 黑板异或游戏 
date: 2022-09-19 16:14:00
updated:
tags: [计算机, 算法, LeetCode, C++, 博弈]
categories: LeetCode题解
cover: https://raw.githubusercontent.com/wsylab/wsyPicGo/main/img/photo-1578662996442-48f60103fc96.avif
description: LeetCode-810「黑板异或游戏」的思考与解答
---
### [LeetCode-810 黑板异或游戏](https://leetcode.cn/problems/chalkboard-xor-game/)

### Solution 1
考虑必胜态, 如果 $nums[i]$ 的异或和为 $0$ , 那么根据规则直接获胜; 否则应该存在一种操作 (移除某个 $nums[i]$ ) 使得下一个状态是必败态. 如果是必败态, 那么不管移走哪个数, 剩下的数的异或和都是 $0$ , 记必败态原本 $nums$ 的异或和为 $XSum$ , 由于去掉一个数的异或和与加上这个数的异或和相等, 故有 $\forall 0\leq i < n, XSum\bigoplus nums[i] = 0$ , 对 $i$ 求异或和, 则有 $\bigoplus_{1\leq t \leq n + 1} XSum = 0$ , 所以必败态 $nums$ 的数量为奇数, 由此倒推, 必胜态 $nums$ 的数量为偶数.
接下来我们证明, 对于一个初始异或和非 $0$ 且元素数量为偶数的数组 $nums$ , 当前玩家总能找到一种操作方式, 使得留给对手的数组是一个初始异或和非 $0$ 且元素数量为奇数的数组. 假设不存在这种选择, 类似前文的证明, 可以推出原数组的元素数量为奇数, 与我们的前提矛盾.
综上, 初始 $nums$ 的异或和为 $0$ 或者数量为偶数时, 先手必胜.
更详细的证明可以参考 [【宫水三叶の相信科学系列】运用「博弈论」分析「先手必胜态」序列具有何种性质](https://leetcode.cn/problems/chalkboard-xor-game/solution/gong-shui-san-xie-noxiang-xin-ke-xue-xi-ges7k/) .
代码如下:
```C++
class Solution {
public:
    bool xorGame(vector<int>& nums) {
        int xor_sum = 0;
        for (auto num: nums) {
            xor_sum = xor_sum ^ num;
        }
        return xor_sum == 0 || !(nums.size() & 1);
    }
};
```  