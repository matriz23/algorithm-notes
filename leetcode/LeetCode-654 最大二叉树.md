---
title: LeetCode-654 最大二叉树 
date: 2022-08-30 09:55:00
updated:
tags: [计算机, 算法, LeetCode, C++, 二叉树, 单调栈]
categories: LeetCode题解
cover: https://raw.githubusercontent.com/wsylab/wsyPicGo/main/img/photo-1524937269764-c14fad76e297
description: LeetCode-654「最大二叉树」的思考与解答
---
### [LeetCode-654 最大二叉树](https://leetcode.cn/problems/maximum-binary-tree/)

### Solution 1
对于数组中的每个元素, 都有一个树节点, 左子树存放左侧最大的树节点, 右子树存放右侧最大的树节点. 使用单调栈来处理. 栈空或当前元素比栈顶小时入栈, 否则出栈至满足前者为止, 一边出栈一边把出栈元素所在节点更新为当前元素的左孩子. 处理完之后如果栈不为空, 那么说明左侧有更大的节点, 当前节点更新为该节点的右孩子.
代码如下:
```C++
class Solution {
public:
    TreeNode* constructMaximumBinaryTree(vector<int>& nums) {
        int n = nums.size();
        vector<int> stk;
        vector<TreeNode*> tree(n);
        for (int i = 0; i < n; ++i) {
            tree[i] = new TreeNode(nums[i]);
            while (!stk.empty() && nums[i] > nums[stk.back()]) {
                tree[i]->left = tree[stk.back()];
                stk.pop_back();
            }
            if (!stk.empty()) {
                tree[stk.back()]->right = tree[i];
            }
            stk.push_back(i);
        }
        return tree[stk[0]];
    }
};
```