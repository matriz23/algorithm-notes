---
title: LeetCode-2561 重排水果 
date: 2023-02-09 23:22:00
updated:
tags: [计算机, 算法, LeetCode, C++, 贪心]
categories: LeetCode题解
cover: 
description: LeetCode-2561「重排水果」的思考与解答
---
### [LeetCode-2561 重排水果](https://leetcode.cn/problems/rearranging-fruits/)

### Solution 1
能使果篮相等的充要条件是每个数出现的次数都是偶数. 
交换可以直接将最大值与最小值交换, 也可以借助最小值为中间值进行交换. 
代码如下:
```C++
#define ll long long
typedef pair<int, int> pii;
class Solution {
   public:
    long long minCost(vector<int>& b1, vector<int>& b2) {
        ll ans = 0;
        map<int, int> bk, bk1, bk2;
        int mc = 1e9 + 10;  // min_cost
        for (int v : b1) {
            bk[v]++;
            bk1[v]++;
            mc = min(mc, v);
        }
        for (int v : b2) {
            bk[v]++;
            bk2[v]++;
            mc = min(mc, v);
        }
        for (auto& v : bk) {
            if (v.second & 1) {
                return -1;
            }
            v.second /= 2;
        }
        vector<int> a1, a2;
        for (auto& v : bk1) {
            if (v.second > bk[v.first]) {
                for (int i = 0; i < v.second - bk[v.first]; i++) {
                    a1.push_back(v.first);
                }
            }
        }
        for (auto& v : bk2) {
            if (v.second > bk[v.first]) {
                for (int i = 0; i < v.second - bk[v.first]; i++) {
                    a2.push_back(v.first);
                }
            }
        }
        sort(a1.begin(), a1.end());
        sort(a2.begin(), a2.end(), greater<int>());
        for (int i = 0; i < a1.size(); i++) {
            ans += min(min(a1[i], a2[i]), 2 * mc);
        }
        return ans;
    }
};
```