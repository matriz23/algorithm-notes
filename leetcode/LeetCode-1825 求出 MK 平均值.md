---
title: LeetCode-1825 求出 MK 平均值 
date: 2023-01-18 11:23:00
updated:
tags: [计算机, 算法, LeetCode, C++, 设计]
categories: LeetCode题解
cover: 
description: LeetCode-1825「求出 MK 平均值」的思考与解答
---
### [LeetCode-1825 求出 MK 平均值](https://leetcode.cn/problems/finding-mk-average/)

### Solution 1
用三个 `multiset` 分别维护容器中最小的 `k` 个数, 中间 `m - 2 * k` 个数以及最大的 `k` 个数.
代码如下:
```C++
#define ll long long
class MKAverage {
private:
    queue<int> q;
    multiset<int> left, mid, right;
    ll sum;
    int m;
    int k;

public:
    MKAverage(int m, int k) {
        sum = 0;
        this->m = m;
        this->k = k;
    }

    void addElement(int num) {
        q.push(num);
        if (q.size() <= m - 2 * k) {
            sum += num;
            mid.insert(num);
            return;
        } else if (q.size() == m + 1) {
            int out = q.front();
            q.pop();
            if (left.count(out)) {
                left.erase(left.find(out));

            } else if (right.count(out)) {
                right.erase(right.find(out));
            } else {
                sum -= out;
                mid.erase(mid.find(out));
                sum += *right.begin();
                mid.insert(*right.begin());
                right.erase(right.begin());
            }
        }
        if (num <= *mid.begin()) {
            left.insert(num);
            if (left.size() > k) {
                right.insert(*mid.rbegin());
                sum -= *mid.rbegin();
                mid.erase(prev(mid.end()));
                sum += *left.rbegin();
                mid.insert(*left.rbegin());
                left.erase(prev(left.end()));
            }
        } else if (num >= *mid.rbegin()) {
            right.insert(num);
            if (right.size() > k) {
                left.insert(*mid.begin());
                sum -= *mid.begin();
                mid.erase(mid.begin());
                sum += *right.begin();
                mid.insert(*right.begin());
                right.erase(right.begin());
            }
        } else {
            mid.insert(num);
            sum += num;
            if (left.size() < k) {
                left.insert(*mid.begin());
                sum -= *mid.begin();
                mid.erase(mid.begin());
            } else {
                right.insert(*mid.rbegin());
                sum -= *mid.rbegin();
                mid.erase(prev(mid.end()));
            }
        }
    }

    int calculateMKAverage() {
        if (q.size() < m) {
            return -1;
        }
        return sum / (m - 2 * k);
    }
};
```