---
title: LeetCode-456 132 模式 
date: 2022-05-09 18:12:00
updated:
tags: [计算机, 算法, LeetCode, C++, 单调栈]
categories: LeetCode题解
cover: https://raw.githubusercontent.com/wsylab/wsyPicGo/main/img/photo-1504394468902-ea647eda589c
description: LeetCode-456「132 模式」的思考与解答
---
### [LeetCode-456 132 模式](https://leetcode.cn/problems/132-pattern/)
这道题运用**单调栈**的解法过于精妙, 特此记录.
### Solution 1
对于三元组 $(i,j,k)$ , 我们考虑枚举 $i$ , 怎么维护 $(j, k)$ 使得 $nums[j] > nums[k]$ 呢? 答案是单调栈. 将 $i$ 右侧第一个数据入栈, 之后遇到比栈顶小的数据就入栈, 否则出栈, 用 $maxk$ 表示出栈元素的最大值, 假如有 $maxk > nums[i]$ , 则必有合法的三元组 $(i,j,k)$ , 这是因为 $maxk$ 出栈是因为有一个更大元素要入栈使得单调减序列不再成立, 这个序列的下标就是我们要找的 $j$ , 而 $maxN$ 的下标就是我们要找的 $k$ .
代码如下 (来自LeetCode用户**宫水三叶**的[题解](https://leetcode.cn/problems/132-pattern/solution/xiang-xin-ke-xue-xi-lie-xiang-jie-wei-he-95gt/)):
```C++
class Solution {
public:
    bool find132pattern(vector<int>& nums) {
        stack<int> st;
        int n = nums.size(), k = INT_MIN;
        for(int i = n - 1; i >= 0; i--){
            if(nums[i] < k) return true;
            while(!st.empty() and st.top() < nums[i]) { 
                k = max(k,st.top()); st.pop();
            }
            st.push(nums[i]);
        }
        return false;
    }
};
```  