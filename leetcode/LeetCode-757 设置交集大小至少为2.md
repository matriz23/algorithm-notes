---
title: LeetCode-757 设置交集大小至少为2 
date: 2022-07-22 10:50:00
updated:
tags: [计算机, 算法, LeetCode, C++, 贪心]
categories: LeetCode题解
cover: https://raw.githubusercontent.com/wsylab/wsyPicGo/main/img/photo-1504554907512-fff129be2012
description: LeetCode-757「设置交集大小至少为2」的思考与解答
---
### [LeetCode-757 设置交集大小至少为2](https://leetcode.cn/problems/set-intersection-size-at-least-two/)

### Solution 1
对于每个区间的被选中的元素, 我们当然希望这些元素也是被其它区间覆盖. 由此可以引出一种贪心的策略: 我们把区间排序, 再从左向右遍历, 遍历时优先选择最右侧的元素. 由于我们优先选择第一个区间最右侧的两个元素, 所以对 $Intervals$ 排序时, 应当按 $Intervals[i][1]$ 升序排序. 如果第二维相同, 如果有左边界最靠右的区间无法利用前面的元素, 这一系列区间的最优选择都与这个区间相同(必然是最优选择之一). 因此第二维升序, 相同时再按第一维降序排列. 之后贪心地选择即可.
代码如下:
```C++
class Solution {
public:
    int intersectionSizeTwo(vector<vector<int>>& intervals) {
        int n = intervals.size();
        
        sort(intervals.begin(), intervals.end(), [&](vector<int> a, vector<int> b) {
            return (a[1] == b[1])? (a[0] > b[0]): (a[1] < b[1]);
        });
        int ans = 2;
        int idx_1 = intervals[0][1] - 1, idx_2 = intervals[0][1];
        for (int i = 1; i < n; i++) {
            if (intervals[i][0] > idx_2) {
                ans += 2;
                idx_1 = intervals[i][1] - 1;
                idx_2 = intervals[i][1];
            }
            else if (intervals[i][0] > idx_1) {
                ans += 1;
                idx_1 = idx_2;
                idx_2 = intervals[i][1];
            }
        }
        return ans;
    }
};
```