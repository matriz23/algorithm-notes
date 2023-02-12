---
title: LeetCode-2334 元素值大于变化阈值的子数组 
date: 2022-08-01 10:09:00
updated:
tags: [计算机, 算法, LeetCode, C++, 并查集, 单调栈]
categories: LeetCode题解
cover: https://raw.githubusercontent.com/wsylab/wsyPicGo/main/img/photo-1515179423979-11466ac39a24
description: LeetCode-2334「元素值大于变化阈值的子数组」的思考与解答
---
### [LeetCode-2334 元素值大于变化阈值的子数组](https://leetcode.cn/problems/subarray-with-elements-greater-than-varying-threshold/)

### Solution 1
按照如下的逻辑思考: 以 $\frac{threshold}{k}$ 为下限, 随着 $k$ 增大, 满足条件的元素会越来越多. 随着元素增多, 联通区域会变大, 如果有一个联通区域长度 $> k$ , 就得到了一个合题意的答案. 这个增加元素/合并区域的过程提示我们使用并查集来处理.
代码如下:
```C++
class Solution {
    typedef pair<int, int> pii;
    vector<int> root, sz; // root 记录连通域最右侧节点, sz 记录连通域大小

    // Union-Find 结构
    int Find(int x) {
        if (root[x] != x) {
            root[x] = Find(root[x]);
        }
        return root[x];
    }

    int Union (int x, int y) { // 合并两个连通区域, 并返回新区域的大小
        x = Find(x);
        y = Find(y);
        if (x == y) {
            return sz[y];
        }
        root[x] = y;
        sz[y] += sz[x];
        return sz[y];
    }

public:
    int validSubarraySize(vector<int>& nums, int threshold) {
        // 随着 k 增大, 能用的数越来越多
        // 我们枚举 k 从 1 - n, 在枚举的过程中对集合进行合并
        int n = nums.size();
        root.resize(n);
        sz.resize(n, 1);
        for (int i = 0; i < n; i++) {
            root[i] = i;
        }
        // 把数组排序, 从大的元素开始考虑, 后面的能够参与连通块时, 前面的也必然可以
        vector<pii> vec;
        for (int i = 0; i < n; i++) {
            vec.emplace_back(nums[i], i);
        }
        sort(vec.begin(), vec.end());
        vector<bool> used(n, false);
        for (int k = 1, pos = n - 1; k <= n; k++) {
            while (pos >= 0 && vec[pos].first > threshold / k) {
                int x = vec[pos].second;
                int len = 1;
                used[x] = true;
                if (x > 0 && used[x - 1]) {
                    len = max(len, Union(x - 1, x));
                }
                if (x < n - 1 && used[x + 1]) {
                    len = max(len, Union(x, x + 1));
                }
                cout<<k<<" "<<len<<endl;
                if (len >= k) { // 联通块的大小 >= k, , 满足了题目定义  
                    return k;
                }
                pos--;
            }
        }
        return -1;
    }
};
```

### Solution 2
同样考虑到联通区域由下限决定, 我们可以考虑以每个元素为最小值的联通区域能有多大, 再判断是不是一个合法的联通区域. 这一部分是经典的单调栈处理左右边界问题.
代码如下:
```C++
class Solution {
public:
    int validSubarraySize(vector<int>& nums, int threshold) {
        int n = nums.size();
        stack<int> s;
        // 处理左侧边界
        vector<int> left(n);
        for (int i = 0; i < n; i++) {
            while (!s.empty() && nums[s.top()] >= nums[i]) {
                s.pop();
            }
            left[i] = s.empty()? -1: s.top();
            s.push(i);
        }
        // 处理右侧边界
        s = stack<int>();
        vector<int> right(n);
        for (int i = n - 1; i >= 0; i--) {
            while (!s.empty() && nums[s.top()] >= nums[i]) {
                s.pop();
            }
            right[i] = s.empty()? n: s.top();
            s.push(i);
        }
        // 寻找合法连通块
        for (int i = 0; i < n; i++) {
            int k = right[i] - left[i] - 1;
            if (nums[i] > threshold / k) {
                return k;
            }
        }
        return -1;
    }
};
```