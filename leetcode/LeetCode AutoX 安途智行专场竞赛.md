---
title: LeetCode AutoX 安途智行专场竞赛 
date: 2022-08-29 17:42:00
updated:
tags: [计算机, 算法, LeetCode, C++]
categories: LeetCode题解
cover: https://raw.githubusercontent.com/wsylab/wsyPicGo/main/img/photo-1508138221679-760a23a2285b
description: LeetCode「AutoX 安途智行专场竞赛」的思考与解答
---
# [LeetCode AutoX 安途智行专场竞赛](https://leetcode.cn/contest/ubiquant2022/)

## Problem 1 网页瀑布流
### 题目链接
[AutoX-1. 网页瀑布流](https://leetcode.cn/contest/autox2023/problems/l9HbCJ/)
### Solution 1
贪心, 用优先队列维护列的高度.
代码如下:
```C++
class Solution {
public:
    int getLengthOfWaterfallFlow(int num, vector<int>& block) {
        priority_queue<int, vector<int>, greater<int>> pq;
        int n = block.size();
        int ans = 0;
        for (int i = 0; i < min(num, n); i++) {
            pq.push(block[i]);
        }
        for (int i = min(num, n); i < n; i++) {
            int top = pq.top();
            pq.pop();
            pq.push(top + block[i]);
        }
        while (!pq.empty()) {
            ans = max(ans, pq.top());
            pq.pop();
        }
        return ans;
    }
};
```
## Problem 2 蚂蚁王国的蜂蜜
### 题目链接
[AutoX-2. 蚂蚁王国的蜂蜜](https://leetcode.cn/contest/autox2023/problems/8p6t8R/)
### Solution 1
用哈希表维护数据即可.
```C++
class Solution {
public:
    vector<double> honeyQuotes(vector<vector<int>>& handle) {
        vector<double> ans;
        int n = 0;
        map<int, int> book;
        for (auto h: handle) {
            int type = h[0];
            if (type == 1) {
                book[h[1]]++;
                n++;
            }
            else if (type == 2) {
                book[h[1]]--;
                n--;
                if (book[h[1]] == 0) {
                    book.erase(h[1]);
                }
            }
            else if (type == 3) {
                if (n == 0) {
                    ans.push_back(-1.0);
                }
                else {
                    double sum = 0;
                    for (auto it = book.begin(); it != book.end(); it++) {
                        sum += (it->first) * (it->second);
                    }
                    ans.push_back(sum / n);
                }
                    
            }
            else if (type == 4) {
                if (n == 0) {
                    ans.push_back(-1.0);
                }
                else {
                    double sum = 0;
                    for (auto it = book.begin(); it != book.end(); it++) {
                        sum += (it->first) * (it->second);
                    }
                    sum = sum / n;
                    double sum2 = 0;
                    for (auto it = book.begin(); it != book.end(); it++) {
                        sum2 += (it->first - sum) * (it->first - sum) * (it->second);
                    }
                    ans.push_back(sum2 / n);
                }

            }
        }
        return ans;
    }
};
```

## Problem 3
### 题目链接
[出行的最少购票费用](https://leetcode.cn/contest/autox2023/problems/BjAFy9/)
### Solution 1
用 $dp[i]$ 记录覆盖 $days[0], ..., days[i]$ 行程的最小费用. 考虑第 $i$ 个行程, 对于每一种 $ticket$ , 最优的选择是把 $ticket$ 的右侧与 $days[i]$ 重合以尽可能覆盖前面的行程, 并检查 ticket 往前第一个覆盖不到的 $days[j]$ , 则有
$$
dp[i] = \underset{ticket\in tickets}{min}dp[j] + ticket[1], j = max\{k\vert dp[i] + 1 - ticket[0] > dp[k]\}
$$
代码如下:
```C++
#define ll long long 
class Solution {
public:
    long long minCostToTravelOnDays(vector<int>& days, vector<vector<int>>& tickets) {
        int n = days.size();
        vector<ll> dp(n, 1e14); 
        for (int i = 0; i < n; i++) {
            for (auto ticket: tickets) {
                int ped = ticket[0];
                ll cost = ticket[1];
                int j = lower_bound(days.begin(), days.end(), days[i] + 1 - ped) - days.begin();
                dp[i] = min(dp[i], (j == 0)? cost: dp[j - 1] + cost);
            }
        }
        return dp[n - 1];
    }
};
``` 

## Problem 4
### 题目链接
[蚂蚁爬行](https://leetcode.cn/contest/autox2023/problems/TcdlJS/)
### Solution 1
并查集维护. 