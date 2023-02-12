---
title: LeetCode-2197 替换数组中的非互质数 
date: 2022-08-30 12:25:00
updated:
tags: [计算机, 算法, LeetCode, C++]
categories: LeetCode题解
cover: https://raw.githubusercontent.com/wsylab/wsyPicGo/main/img/photo-1455577380025-4321f1e1dca7
description: LeetCode-2197「替换数组中的非互质数」的思考与解答
---
### [LeetCode-2197 替换数组中的非互质数](https://leetcode.cn/problems/replace-non-coprime-numbers-in-array/)

### Solution 1
一个直接的想法是, 从左向右遍历, 再从右向左遍历, 不断合并, 直到数组不再改变为止.
代码如下:
```C++
#define ll long long
class Solution {
public:
    vector<int> replaceNonCoprimes(vector<int>& nums) {       
        int cnt = 1;
        while (1) {
            vector<int> res;
            int n  = nums.size();
            if (cnt % 2 == 1) {
                ll lcm = nums[0];
                for (int i = 1; i < n; i++) {
                    if (__gcd(lcm, (ll)nums[i]) == 1) {
                        res.push_back(lcm);
                        lcm = nums[i];
                    }
                    else {
                        lcm = lcm * nums[i] / __gcd(lcm, (ll)nums[i]);
                    }
                }
                res.push_back(lcm);
                nums = res;
            }
            else {
                ll lcm = nums[n - 1];
                for (int i = n - 2; i >= 0; i--) {
                    if (__gcd(lcm, (ll)nums[i]) == 1) {
                        res.push_back(lcm);
                        lcm = nums[i];
                    }
                    else {
                        lcm = lcm * nums[i] / __gcd(lcm, (ll)nums[i]);
                    }
                }
                res.push_back(lcm);
                nums.clear();
                for (int i = res.size() - 1; i >= 0; i--) {
                    nums.push_back(res[i]);
                }
            }
            if (nums.size() == n) {
                return nums;
            }
            cnt++;
        }
    }
};
```

### Solution 2
对于这种与两侧元素交互的数组修改问题, 可以考虑使用栈来处理. 从左至右遍历元素, 每次取出栈顶进行合并直到不能合并为止. 最终栈中的元素就是答案.
代码如下:
```C++
#define ll long long
class Solution {
public:
    vector<int> replaceNonCoprimes(vector<int>& nums) {       
        stack<int> st;
        for (int &num: nums) {
            while (!st.empty() && __gcd(st.top(), num) != 1) {
                num = (ll)num * st.top() / __gcd(st.top(), num);
                st.pop();
            }
            st.push(num);
        }
        vector<int> ans(st.size());
        for (int i = ans.size() - 1; i >= 0; i--) {
            ans[i] = st.top();
            st.pop();
        }
        return ans;
    }
};
```