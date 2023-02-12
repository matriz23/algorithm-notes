---
title: LeetCode-2271 毯子覆盖的最多白色砖块数 
date: 2022-08-23 17:03:00
updated:
tags: [计算机, 算法, LeetCode, C++, 贪心, 双指针, 滑动窗口]
categories: LeetCode题解
cover: https://raw.githubusercontent.com/wsylab/wsyPicGo/main/img/photo-1594922234647-4ade6282c369.avif
description: LeetCode-2271「毯子覆盖的最多白色砖块数」的思考与解答
---
### [LeetCode-2271 毯子覆盖的最多白色砖块数](https://leetcode.cn/problems/maximum-white-tiles-covered-by-a-carpet/)

### Solution 1
地毯覆盖最多白色砖块时, 它的左端点一定处于某个白色砖块区间的左端点. 想到这一点, 我们用双指针遍历所有可能. 一个指针用于记录左侧端点, 另一个指针记录右侧最后一个有重合的区间左端点.
代码如下:
```C++
#define ll long long
class Solution {
public:
    int maximumWhiteTiles(vector<vector<int>>& tiles, int carpetLen) {
        sort(tiles.begin(), tiles.end());
        ll now = 0, ans = 0;
        for (int i = 0, j = 0; i < tiles.size(); i++) {
            while (j < tiles.size() && tiles[j][1] + 1 - tiles[i][0] <= carpetLen) now += tiles[j][1] - tiles[j][0] + 1, j++; // 计算完全被覆盖的区间
            // 毯子无法完全覆盖第 j 组瓷砖
            if (j < tiles.size()) ans = max(ans, now + max(0, tiles[i][0] + carpetLen - tiles[j][0]));
            // 毯子可以完全覆盖第 j 组瓷砖
            else ans = max(ans, now);
            now -= tiles[i][1] - tiles[i][0] + 1;
        }
        return ans;
    }
};
```