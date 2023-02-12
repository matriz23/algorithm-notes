---
title: LeetCode-390 消除游戏
date: 2022-05-04 17:13:00
updated:
tags: [计算机, 算法, LeetCode, C++, 数学, 动态规划, 对称性]
categories: LeetCode题解
cover: https://raw.githubusercontent.com/wsylab/wsyPicGo/main/img/photo-1587654780291-39c9404d746b
description: LeetCode-390「消除游戏」的思考与解答
---
### [LeetCode-390 消除游戏](https://leetcode.cn/problems/elimination-game/)

### Solution 1
这一题的形式很容易让人想到约瑟夫环问题, 事实上, 这道题也有巧妙的数学解法.
题中的操作是先从左到右删除元素, 再从右到左删除元素, 如此反复直至数列只剩一个元素, 对于一个数列 $1,2,...,i$ , 我们把题给操作下最终保留的元素记作 $F(i)$ ; 现在考虑其对称操作: 先从右至左删除元素, 再从左至右删除元素, 如此反复直至数列只剩一个元素, 同样地, 我们给这一操作最终保留的元素一个记号, 记作 $G(i)$ .
由于这两种操作具备完全的对称性, 故有 
$$
F(i) + G(i) = 1 + i
$$

由于我们最终的目标是找到 $F(i)$ 的一个递推式, 现在需要消去 $G(i)$ . 对数列 $1,2,...,i$ , 考虑第一步从左至右删除元素, 得到数列$2, 4,..., \lfloor i/2 \rfloor$ , 我们想要对新得到的数列进行 $G$ 操作, 由于 $G(n)$ 是对 $1,2,...,n$ 进行的操作, 我们先建立起 $2, 4,...,2× \lfloor i/2 \rfloor$ 与 $1,2,...,k$ 的联系, 容易发现 $x_{j}^{new} =  x_{j}^{old} /2$ , 故对 $2, 4,...,2× \lfloor i/2 \rfloor$ 进行 $G$ 操作之后所得的数应为 $2×G( \lfloor i /2  \rfloor )$ , 即有

$$
F(i) = 2 × G( \lfloor i /2 \rfloor ) 
$$
得到了 $F(n)$ 与 $G(n)$ 的两个公式, 消去 $G(n)$ 可以得到仅含有 $F(n)$ 的递推式:
$$
F(i) = 2 × (1 + \lfloor i/2 \rfloor - F(\lfloor i/2 \rfloor))
$$
边界情况为 $F(1) = 1$ .
代码如下:
```C++
class Solution {
    public int lastRemaining(int n) {
        return n == 1 ? 1 : 2 * (n / 2 + 1 - lastRemaining(n / 2));
    }
}
```