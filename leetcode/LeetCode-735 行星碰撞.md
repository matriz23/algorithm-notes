---
title: LeetCode-735 行星碰撞 
date: 2022-07-13 10:55:00
updated:
tags: [计算机, 算法, LeetCode, C++, 栈, 模拟, 数据结构]
categories: LeetCode题解
cover: https://raw.githubusercontent.com/wsylab/wsyPicGo/main/img/photo-1469980098053-382eb10ba017
description: LeetCode-735「行星碰撞」的思考与解答
---
### [LeetCode-735 行星碰撞](https://leetcode.cn/problems/asteroid-collision/)

### Solution 1
一颗向右的行星总是能摧毁比它小的向左的行星(如果目标星球在这之前被摧毁了, 也可以视作被该行星摧毁), 向左的同理. 考虑用一个数据结构模拟行星运动的过程, 我们可以从左向右遍历, 同时维护一个栈 $st$ , 用 $st$ 存储向右的行星, 如果遇到向左的行星, 一直执行 $pop()$ 操作直到栈顶元素大于行星即可. 一颗向左的行星只有在栈空时才能存活.
代码如下:
```C++
class Solution {
public:
    vector<int> asteroidCollision(vector<int>& asteroids) {
        int n = asteroids.size();
        vector<int> ans;
        // priority_queue<int> pq;
        stack<int> st;
        for (int i = 0; i < n; i++) {
            if (asteroids[i] < 0) {
                if (st.empty()) {
                    ans.push_back(asteroids[i]);
                }
                else {
                    while (st.top() < -asteroids[i]) {
                        st.pop();
                        if (st.empty()) {
                            ans.push_back(asteroids[i]);
                            break; // 只有把向右的全部摧毁, 才能存活
                        }
                    }
                    if (!st.empty()) {
                        if (st.top() + asteroids[i] == 0) {
                            st.pop(); // 同归于尽
                        }
                    }
                }
            }
            else {
                st.push(asteroids[i]);
            }
        }
        // st中存储着存活到最后的向右行星
        stack<int> res; // 用res栈把st栈中的元素颠倒一下
        while (!st.empty()) {
            res.push(st.top());
            st.pop();
        }
        while (!res.empty()) {
            ans.push_back(res.top());
            res.pop();
        } 
        return ans;
    }
};
```