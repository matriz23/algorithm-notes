---
title: LeetCode-857 雇佣 K 名工人的最低成本 
date: 2022-09-11 19:18:00
updated:
tags: [计算机, 算法, LeetCode, C++, 贪心]
categories: LeetCode题解
cover: https://raw.githubusercontent.com/wsylab/wsyPicGo/main/img/photo-1589939705384-5185137a7f0f.avif
description: LeetCode-857「雇佣 K 名工人的最低成本」的思考与解答
---
### [LeetCode-857 雇佣 K 名工人的最低成本](https://leetcode.cn/problems/minimum-cost-to-hire-k-workers/)

### Solution 1
定义价性比为 $\frac{wage[i]}{quality[i]}$. 根据题意可以知道, $k$ 个工人的总工资由最高的价性比乘以全组质量之和. 因此我们从低到高枚举价性比, 同时用一个优先队列维护当前前 $k$ 小的 $quality$ .
代码如下:
```C++
class Solution {
public:
    typedef pair<double, int> pdi;
    double mincostToHireWorkers(vector<int>& quality, vector<int>& wage, int k) {
        int n = quality.size();
        vector<pdi> avg_wage;
        for (int i = 0; i < n; i++) {
            double a_w = (double)wage[i] / (double)quality[i];
            avg_wage.emplace_back(a_w, i);
        }
        sort(avg_wage.begin(), avg_wage.end());
        double ans = 1e9;
        priority_queue<int> pq;
        int k_sum = 0;
        for (int i = 0; i < k; i++) {
            pq.push(quality[avg_wage[i].second]);
            k_sum += quality[avg_wage[i].second];
        }
        ans = k_sum * avg_wage[k - 1].first;
        for (int i = k; i < n; i++) {
            if (pq.top() > quality[avg_wage[i].second]) {
                k_sum = k_sum - pq.top() + quality[avg_wage[i].second];
                pq.pop();
                pq.push(quality[avg_wage[i].second]);
                ans = min(ans, avg_wage[i].first * k_sum);
            }        
        }
        return ans;
    }
};
```