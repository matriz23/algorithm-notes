---
title: LeetCode-2333 最小差值平方和 
date: 2022-07-11 18:03:00
updated: 2022-07-11 23:20:00
tags: [计算机, 算法, LeetCode, C++, 二分, 模拟, 贪心]
categories: LeetCode题解
cover: https://raw.githubusercontent.com/wsylab/wsyPicGo/main/img/photo-1478088913771-e3a36f50bb63
description: LeetCode-2333「最小差值平方和」的思考与解答
---
### [LeetCode-2333 最小差值平方和](https://leetcode.cn/problems/minimum-sum-of-squared-difference/)

### Solution 1
我们考虑 $nums1$ 与 $nums2$ 的差的绝对值数组 $d$ , 注意到对 $nums1$ 的操作与 $nums2$ 的操作没有区别, 都等价于对 $d$ 操作. 这道题转化成有 $k = k_1 + k_2$ 次操作, 使得 $d$ 的元素平方和最小. 容易发现应该优先处理大的元素, 一个常规的想法是使用优先队列维护当前数列, 每次取出最大的元素处理后再入列, 直到操作次数用完. 这种算法没有问题, 但时间复杂度太高.
把数组想成不断上升的山脉, 我们做的事情就是不停地把山峰"削平", 显然最终的状态必然是数列末尾连续的一些山峰高度相同. 我们考虑二分搜索这个最终高度 $t$ . 把大于 $t$ 的数变为 $t$ 之后可能会剩下一些操作次数, 我们再进行模拟.
代码如下:
```C++
class Solution {
public:
    // isValid()函数判断一个给定的t是否能在给定操作数内达到
    bool isValid(vector<long long> d, long long cnt, long long t) {
        int n = d.size();
        // 寻找idx的过程也可以通过二分进一步优化
        int idx = -1;
        for (int i = n - 1; i >= 0; i--) {
            if (d[i] >= t) {
                idx = i;
            }
            else {
                break;
            }
        }
        if (idx == -1) {
            return true;
        }
        for (int i = n - 1; i >= idx; i--) {
            cnt -= d[i] - t;
        }
        return cnt >= 0? true: false;
    }

    long long minSumSquareDiff(vector<int>& nums1, vector<int>& nums2, int k1, int k2) {
        int n = nums1.size();
        long long cnt = k1 + k2;
        vector<long long> d(n, 0);
        for (int i = 0; i < n; i++) {
            d[i] = abs(nums1[i] - nums2[i]);
        }
        sort(d.begin(), d.end()); // 对d排序, 方便后续处理

        long long left = 0;
        long long right = d[n - 1] + 1; // 二分搜索目标t
        while (left < right) {
            long long mid = left + (right - left) / 2;
            if (isValid(d, cnt, mid)) {
                right = mid;
            }
            else {
                left = mid + 1;
            }
        }
    
        // 找到可行的最小的t, 先把大于t的数减到t, 后续再处理剩余的操作数
        long long t = left;
        for (int i = n - 1; i >= 0; i--) {
            if (d[i] <= t) {
                break;
            }
            int temp = d[i] - t;
            if (cnt - temp >= 0) {
                cnt -= temp;
                d[i] = t;
            }
            else {
                d[i] -= cnt;
                break;
            }
        }

        long long ans = 0;
        if (cnt > 0) { // 有多余操作数, 我们建立优先队列来模拟
            priority_queue<long long> q;
            for (int i = 0; i < n; i++) {
                q.push(d[i]);
            }
            while (cnt > 0 && q.top() > 0) {
                long long temp = q.top();
                q.pop();
                temp -= 1;
                cnt--;
                q.push(temp);
            }
            while (!q.empty()) { // 因为是在优先队列里进行的操作, 所以我们在里直接计算ans
                long long temp = q.top();
                ans += temp * temp;
                q.pop();
            }
            return ans;
        }
        // 没有多余操作数, 通过数组d计算ans
        for (int i = 0; i < n; i++) {
            ans += d[i] * d[i];
        }
        return ans;
    }
};
```

## Solution 2
二分的写法还是比较繁琐, 我们考虑模拟的过程能否优化? 对于"削峰"的操作, 观察后可以发现结果相当于同时减少末尾相等的那些数. 如果把这一过程统一操作而不是在优先队列里一个个单独操作, 将会大大减少时间消耗.
我们把差分数组 $d$ 从大到小排序, 当遍历至 $a[i]$ 时, 需要把 $a[0]$ 到 $a[i]$ 都减小到 $a[i + 1]$ , 在这里判断所需的操作数 $cnt$ 与当前剩余操作数 $k$ 的大小关系. 当 $k \geq cnt$ 时, 正常操作; 如果 $k < cnt$ , 则 $cnt\ mod\ (i + 1)$ 个能减小 $\lfloor \frac{cnt}{i + 1} \rfloor + 1$, 而剩余的 $i + 1 - cnt\ mod\ (i + 1)$ 能减小 $\lfloor \frac{cnt}{i + 1} \rfloor$ . 为了统一 $i = n - 1$ 时的操作, 我们在数组末尾添加一个 $0$ 即可.
代码如下:
```C++
class Solution {
public:
    long long minSumSquareDiff(vector<int>& nums1, vector<int>& nums2, int k1, int k2) {
        long long ans = 0;
        int n =nums1.size();
        int k = k1 + k2;
        vector<long long> d(n, 0);
        for (int i = 0; i < n; i++) {
            d[i] = abs(nums1[i] - nums2[i]);
            ans += d[i] * d[i];
        }
        sort(d.begin(), d.end(), greater<int>());
        d.push_back(0); // 增加一个哨兵0, 统一操作
        for (int i = 0; i < n; i++) {
            int j = i + 1;
            long long cnt = j * (d[i] - d[i + 1]);
            ans -= d[i] * d[i]; // 先把处理过的减掉, 最后再统一加起来
            if (k >= cnt) { // 操作数够用, 就把cnt减掉
                k -= cnt;
                continue;
            }
            else { // 操作数不够用了, 把之前抹平的加回去
                ans += k % j * (d[i] - k / j - 1) * (d[i] - k / j - 1) + (j - k % j) * (d[i] - k / j) * (d[i] - k / j);
                break;
            }
        }
        return ans;
    }
};
```