---
title: LeetCode-871 最低加油次数 
date: 2022-07-02 15:37:00
updated:
tags: [计算机, 算法, LeetCode, C++, 贪心, 优先队列]
categories: LeetCode题解
cover: https://raw.githubusercontent.com/wsylab/wsyPicGo/main/img/photo-1635627026254-b652e62d1d07
description: LeetCode-871「最低加油次数」的思考与解答
---
### [LeetCode-871 最低加油次数](https://leetcode.cn/problems/minimum-number-of-refueling-stops/)

### Solution 1
这道题与[CodeForces-1926C2 Potions (Hard Version)](https://codeforces.com/problemset/problem/1526/C2)非常相似, 虽然本题求的是最少加油次数, 而后者求的是最多喝药次数, 但思考逻辑是相同的.
需要尽可能少地加油, 我们考虑尽可能不加油, 计算到下一站的油量. 当油量不够时, 再从之前路过而未加加油站中选择油量最大的加上. 如果加完了油还不足以到达下一站, 则返回 $-1$ . 把终点也视作一个加油站, 便于统一处理.
代码如下:
```C++
class Solution {
public:
    int minRefuelStops(int target, int startFuel, vector<vector<int>>& stations) {
        int ans = 0;
        vector<int> end;
        end.push_back(target);
        end.push_back(0);
        stations.push_back(end);
        priority_queue<long long> q;
        int n = stations.size();
        int fuel = startFuel;
        for (int i = 0; i < n; i++) {
            fuel -= (i == 0)? stations[i][0]: stations[i][0] - stations[i - 1][0];
            if (fuel < 0) {
                while (!q.empty() && fuel < 0) {
                    int addFuel = q.top();
                    q.pop();
                    fuel += addFuel;
                    ans++;
                }
                if (fuel < 0) {
                    return -1;
                }
            }
            q.push(stations[i][1]);
        }
        return ans;
    }
};
```