---
title: LeetCode-765 情侣牵手 
date: 2022-09-12 19:02:00
updated:
tags: [计算机, 算法, LeetCode, C++, 贪心, 并查集]
categories: LeetCode题解
cover: https://raw.githubusercontent.com/wsylab/wsyPicGo/main/img/photo-1517607648415-b431854daa86.avif
description: LeetCode-765「情侣牵手」的思考与解答
---
### [LeetCode-765 情侣牵手](https://leetcode.cn/problems/couples-holding-hands/)

### Solution 1
贪心, 遇到不匹配的就交换.
代码如下:
```C++
class Solution {
public:
    int minSwapsCouples(vector<int>& row) {
        int n = row.size();
        map<int, int> idx;
        for (int i = 0; i < n; i++) {
            idx[row[i]] = i;
        }
        int ans = 0;
        for (int i = 0; i < n; i += 2) {
            if (row[i] / 2 != row[i + 1] / 2) {
                int id = (row[i] % 2 == 0)? idx[row[i] + 1]: idx[row[i] - 1];
                idx[row[i + 1]] = id;
                row[id] = row[i + 1];
                ans++;
            }
        }
        return ans;
    }
};
```

### Solution 2
同一对情侣的编号除以 $2$ 之后相同, 不妨把这个数看作情侣对的编号. 考虑 $\frac{n}{2}$ 个情侣节点, 如果存在 $2k, 2k + 1$ 的位置上是不同对的情侣, 就连一条边. 最终图中节点一定分成了若干个环. 一个长度为 $len$ 的环需要 $len - 1$ 次交换才能分开. 最终需要的次数 $= \sum_{i = 1}^{t})(len_i - 1) = \frac{n}{2} - t$ .
具体实现时, 我们利用并查集来处理环的结构. 最终 $i = pa[i]$ 的节点就是一个环. 
代码如下: 
```C++
class Solution {
public:
    int pa[70];

    void init() {
        for (int i = 0; i < 70; i++) {
            pa[i] = i;
        }
    }

    int find(int x) {
        if (pa[x] == x) {
            return x;
        }
        pa[x] = find(pa[x]);
        return pa[x];
    }

    void merge(int x, int y) {
        pa[find(y)] = find(x); 
    }

    int minSwapsCouples(vector<int>& row) {
        init();
        int n = row.size();
        for (int i = 0; i < n; i += 2) {
            merge(row[i] / 2, row[i + 1] / 2);
        }
        int cnt = 0;
        for (int i = 0; i < n / 2; i++) {
            if (i == find(i)) {
                cnt++;
            }
        }
        return n / 2 - cnt;
    }
};
```