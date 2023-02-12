---
title: LeetCode-1823 找出游戏的获胜者
date: 2022-05-04 12:47:00
updated:
tags: [计算机, 算法, LeetCode, C++, 数学, 动态规划, 模拟]
categories: LeetCode题解
cover: https://raw.githubusercontent.com/wsylab/wsyPicGo/main/img/photo-1650825733710-1ab249421197
description: LeetCode-1823 「找出游戏的获胜者」的思考与解答
---
### [LeetCode-1823 找出游戏的获胜者](https://leetcode.cn/problems/find-the-winner-of-the-circular-game/)
本题是一道非常经典的问题: ["约瑟夫环问题"](https://baike.baidu.com/item/%E7%BA%A6%E7%91%9F%E5%A4%AB%E9%97%AE%E9%A2%98). 
### Solution 1
在大一的编程课上就做过这道题, 当时用的是模拟的方法. 
这道题给人的第一印象就是一道模拟题. 我们用一个 $bool$ 数组来模拟出局情况, 初始都设定为 $true$ ,当有人出局时将其修改为 $false$ .
首先注意到每一轮必定有一人出局, 初始有 $n$ 人, 最后剩余 $1$ 人, 所以游戏一共经历了 $n-1$ 轮. 我们利用一个指针 $p$ 来模拟点名的状况, 为了防止数组越界, 考虑对 $p$ 取模来模拟"循环"的过程.
最后不要忘记题目中序号从 $1$ 开始, 而数组中从 $0$ 开始, 返回的答案应该取模后 $+1$.
代码如下: 
```C++
class Solution {
public:
    int findTheWinner(int n, int k) {
        // 0,...,n - 1
        vector<bool> isLive(n, true);
        int p = 0;
        for (int i = 0; i < n - 1; i++) {
            for (int j = 0; j < k; j++) {
                while (!isLive[p % n]){ //寻找下一个未出局的人
                    p++;
                }
                if (j != k -1) {
                    p++;
                } // 在前k - 1次点名中, 点名后直接跳过, 而第k次点名到的人要出局
            }
            isLive[p % n] = false;
            p++;
        }
        while (!isLive[p % n]){
            p++;
        }
        return p % n + 1; // 输出时返回原来的序号
    }
};
```

### Solution 2
这道题的数学模型相对简洁, 我们可以直接从数学上入手. 设 $P(n, k)$ 是初始 $n$ 人, 每次点名 $k$ 人最终的获胜者, 考虑其与 $P(n - 1, k)$ 的关系. 对于 $P(n, k)$ ,第一次点名结束后就能到达了 $P(n - 1, k)$ 的情况, 只不过序号发生的变化: 每个存活者的序号都前移了 $k$ 位( $mod\ n$ 的意义下), 故可以得到如下的递推关系:
$$
P(n, k) = (P(n - 1, k) + k)\ mod\ n
$$
注意到每个状态只和它前一个状态有关, 用两个变量即可完成递推.
代码如下:
```C++
class Solution {
public:
    int findTheWinner(int n, int k) {
        int p_0 = 0;
        int p_1 = 0;
        for (int i = 1; i < n; i++) {
            p_1 = (p_0 + k) % (i + 1);
            p_0 = p_1;
        }
        return p_1 + 1;
    }
};
```