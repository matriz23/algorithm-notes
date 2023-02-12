---
title: LeetCode-2454 下一个更大元素 IV 
date: 2022-11-09 14:17:00
updated:
tags: [计算机, 算法, LeetCode, C++]
categories: LeetCode题解
cover: 
description: LeetCode-2454「下一个更大元素 IV」的思考与解答
---
### [LeetCode-2454 下一个更大元素 IV](https://leetcode.cn/problems/next-greater-element-iv/)

### Solution 1
在寻找 "下一个更大元素" 时, 通常利用单调栈处理. 现在需要寻找 "下面第二个更大元素" , 原先的处理思路需要变化. 我们考虑对一个元素找两次 "下一个更大元素" . 申请两个栈 $s_1$ 和 $s_2$ , 第一个依照寻找 "下一个更大元素" 正常处理, 对于找到了下一个最大元素的元素, 我们把它们按照原来的顺序存到 $s_2$ 中. 这样遍历数组时, $s_1$ 弹出的将会存到 $s_2$ 中, 而 $s_2$ 弹出的就是在右侧找到了两个最大元素的元素. 
代码如下:
```C++
class Solution {
public:
    vector<int> secondGreaterElement(vector<int>& nums) {
        int n = nums.size();
        vector<int> ans(n, -1);
        stack<int> s1, s2;
        for (int i = 0; i < n; i++) {
            stack<int> temp;
            while (!s1.empty() && nums[i] > nums[s1.top()]) {
                temp.push(s1.top());
                s1.pop();
            }
            s1.push(i);
            while (!s2.empty() && nums[i] > nums[s2.top()]) {
                ans[s2.top()] = nums[i];
                s2.pop();
            }
            while (!temp.empty()) {
                s2.push(temp.top());
                temp.pop();
            }
        }
        return ans;
    }
};
```