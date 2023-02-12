---
title: LeetCode-56 合并区间 
date: 2022-05-15 13:20:00
updated:
tags: [计算机, 算法, LeetCode, C++, 区间合并, 贪心]
categories: LeetCode题解
cover: https://raw.githubusercontent.com/wsylab/wsyPicGo/main/img/photo-1561588860-6a3aaf1feb3e
description: LeetCode-56「合并区间」的思考与解答
---
### [LeetCode-56 合并区间](https://leetcode.cn/problems/merge-intervals/)

### Solution 1
给定一组区间, 要求合并区间, 首先对区间按照起始端点进行排序. 考虑什么样的区间需要合并, 可以发现当区间右端点大于下一个区间左端点时, 需要进行区间合并, 我们把下一个区间改写成合并后的区间. 合并时需要注意合并区间的左端点总是当前区间的左端点, 而右端点是两个区间的最大值.
注意这种做法当最后一个端点单独向后合并时需要另外处理.
代码如下:
```C++
class Solution {
public:
    vector<vector<int>> merge(vector<vector<int>>& intervals) {
        sort(intervals.begin(), intervals.end());
        vector<vector<int>> merge;
        int p = 0;
        while (p < intervals.size() - 1) {
            while (p < intervals.size() - 1 && intervals[p][1] >= intervals[p + 1][0]) {
                intervals[p + 1][0] = intervals[p][0];
                intervals[p + 1][1] = max(intervals[p + 1][1], intervals[p][1]);
                p++;
            }
            merge.push_back(intervals[p]);
            p++;
        }
        if (merge.size() == 0) {
            merge.push_back(intervals.back());
        }
        if (merge.back() != intervals.back()) {
            merge.push_back(intervals.back());
        }
        return merge;
    }
};
```