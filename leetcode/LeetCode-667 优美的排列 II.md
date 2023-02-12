---
title: LeetCode-667 优美的排列 II 
date: 2022-09-08 11:54:00
updated:
tags: [计算机, 算法, LeetCode, C++]
categories: LeetCode题解
cover: https://raw.githubusercontent.com/wsylab/wsyPicGo/main/img/photo-1429081172764-c0ee67ab9afd
description: LeetCode-667「优美的排列 II」的思考与解答
---
### [LeetCode-667 优美的排列 II](https://leetcode.cn/problems/beautiful-arrangement-ii/)

### Solution 1
对于 $1, 2, ..., n$ , 要求构造出 $k$ 个差值, 我们不妨直接取 $1, 2, ..., k$ 这 $k$ 个差值. 先构造出最大的, 把 $n$ 插入到 $n - k$ 和 $n - k + 1$ 中间, 则数组转化为 $1, ..., n - k, n, n - k + 1, ..., n - 1$ , 已经拥有了 $k$ 和 $k - 1$ 这两个差值. 对于 $n - k + 1, ..., n - 1$ 这个数组, 有 $k - 1$ 个连续元素, 需要构造出 $1, 2, ..., k - 2$ 这 $k - 2$ 个差值, 很明显是一个规模更小的子问题. 注意到我们的构造方式不会影响变动数组首元素的位置, 所以后续构造对前面没有影响. 递归地构造即可.
代码如下:
```C++
class Solution {
public:
    vector<int> ans;
    // 连续元素构造, start, start + 1, ..., end, 构造出  num 个差值
    void constructAns(int start, int end, int num) {
        if (num < 0) { // 递归终点
            return; 
        }
        for (int i = start; i <= end - num; i++) {
            ans.push_back(i);
        }
        if (num != 0) { // num == 0 时, start end 相同, 不需要再加入 end
            ans.push_back(end);
        }
        // 规模更小的子问题
        constructAns(end - num + 1, end - 1, num - 2);
        return;
    }
    vector<int> constructArray(int n, int k) {
        constructAns(1, n, k);
        return ans;
    }
};
```

### Solution 2
找规律, 前面部分差值为 $1$ , 后面部分大小交替, 差值从 $k$ 递减到 $1$ , 即 $[1, 2, ⋯, n−k, n, n−k+1, n−1, n−k+2, ⋯]$
```C++
class Solution {
public:
    vector<int> constructArray(int n, int k) {
        vector<int> answer;
        for (int i = 1; i < n - k; ++i) {
            answer.push_back(i);
        }
        for (int i = n - k, j = n; i <= j; ++i, --j) {
            answer.push_back(i);
            if (i != j) {
                answer.push_back(j);
            }
        }
        return answer;
    }
};
```
