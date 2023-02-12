---
title: LeetCode-975 奇偶跳 
date: 2022-09-13 22:56:00
updated:
tags: [计算机, 算法, LeetCode, C++, 动态规划, 单调栈]
categories: LeetCode题解
cover: https://raw.githubusercontent.com/wsylab/wsyPicGo/main/img/photo-1662668862763-dc613ee191ab.avif
description: LeetCode-975「奇偶跳」的思考与解答
---
### [LeetCode-975 奇偶跳](https://leetcode.cn/problems/odd-even-jump/)

### Solution 1
本题的关键在于怎么找到每个元素的后继. 以 $a[i] <= a[j], a[j]$ 为可能的最小值, $j$ 为可能的最小值中最小的下标为例. 快速地找到每个 $i$ 对应的 $j$ 是一大难点. 
这种结构很容易让人想到应用单调栈解决的一个经典问题: 找到每个元素后第一个不小于自己的元素下标. 事实上, 我们把下标和元素值抽象成两个属性 $A$ 和 $B$ . 那么已经解决的问题就是寻找 $B$ 不小于当前元素, $A$ 不小于当前元素且 $A$ 尽可能小的元素; 我们要解决的问题就是寻找 $A$ 不小于当前元素, $B$ 不小于当前元素且 $B$ 尽可能小的元素 (如果存在多个 $B$ 相同的元素, 那么 $A$ 也要尽可能小). 可以看出这两个问题是基本对称的( $A$ 作为下标时不可能重复, 如果可重那么两个问题完全对称). 我们之前从小到大遍历 $A$ 属性(下标), 同时维护 $B$ 属性单调的栈; 那么现在从小到大遍历 $B$ 属性, 同时维护 $A$ 属性单调的栈即可.
代码如下:
```C++
class Solution {
public:
    typedef pair<int, int> pii;
    int oddEvenJumps(vector<int>& arr) {
        int n = arr.size();
        vector<int> high(n), low(n);
        vector<pii> vec;
        for (int i = 0; i < n; i++) {
            vec.emplace_back(i, arr[i]);
        }
        sort(vec.begin(), vec.end(), [](pii &a, pii &b) {
            return a.second == b.second? a.first < b.first: a.second < b.second;
        });
        stack<int> s;
        for (int i = 0; i < n; i++) {
            while (!s.empty() && s.top() < vec[i].first) {
                high[s.top()] = vec[i].first;
                s.pop();
            }
            s.push(vec[i].first);
        }
        while (!s.empty()) {
            high[s.top()] = n;
            s.pop();
        }
        sort(vec.begin(), vec.end(), [](pii &a, pii &b) {
            return a.second == b.second? a.first < b.first: a.second > b.second;
        });
        for (int i = 0; i < n; i++) {
            while (!s.empty() && s.top() < vec[i].first) {
                low[s.top()] = vec[i].first;
                s.pop();
            }
            s.push(vec[i].first);
        }
        while (!s.empty()) {
            low[s.top()] = n;
            s.pop();
        }
        vector<vector<bool>> dp(n + 1, vector<bool>(2, false));
        dp[n - 1][0] = true;
        dp[n - 1][1] = true;
        for (int i = n - 2; i >= 0; i--) {
            dp[i][0] = dp[high[i]][1];
            dp[i][1] = dp[low[i]][0];
        }
        int ans = 0;
        for (int i = 0; i < n; i++) {
            ans += dp[i][0];
        }
        return ans;
    }
};
```