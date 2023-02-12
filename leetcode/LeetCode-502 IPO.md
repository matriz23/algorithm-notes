---
title: LeetCode-502 IPO
date: 2022-09-05 16:21:00
updated:
tags: [计算机, 算法, LeetCode, C++, 二分, 贪心]
categories: LeetCode题解
cover: https://raw.githubusercontent.com/wsylab/wsyPicGo/main/img/photo-1604689598793-b8bf1dc445a1.avif
description: LeetCode-502「IPO」的思考与解答
---
### [LeetCode-502 IPO](https://leetcode.cn/problems/ipo/)

### Solution 1
当前拥有资本 $w$ , 下一步选择肯定是启动资本不超过 $w$ 的项目中利润最大的那一个. 每次先二分搜索找到所有可行项目存入优先队列中, 再取队首元素更新即可.
代码如下:
```C++
typedef pair<int, int> pii;
class Solution {
public:
    struct cmp1 {
        bool operator()(const pii &a, const pii &b) {
            return (a.second == b.second)? a.first < b.first: a.second < b.second;
        }
    };
    int findMaximizedCapital(int k, int w, vector<int>& profits, vector<int>& capital) {
        vector<pii> projects;
        int n = profits.size();
        for (int i = 0; i < n; i++) {
            projects.emplace_back(capital[i], profits[i]);
        }
        sort(projects.begin(), projects.end());
        priority_queue<pii, vector<pii>, cmp1> pq;
        int idx = -1;
        while (k--) {
            int left = 0;
            int right = n;
            while (left < right) {
                int mid = left + (right - left) / 2;
                if (projects[mid].first <= w) {
                    left = mid + 1;
                }
                else {
                    right = mid;
                }
            }
            for (int i = idx + 1; i <= left - 1; i++) {
                pq.push(projects[i]);
            }
            idx = left - 1;
            if (pq.empty()) {
                break;
            }
            w += pq.top().second;
            pq.pop();
        }
        return w;
    }
};
```