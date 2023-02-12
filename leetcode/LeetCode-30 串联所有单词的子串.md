---
title: LeetCode-30 串联所有单词的子串 
date: 2022-06-23 23:10:00
updated:
tags: [计算机, 算法, LeetCode, C++, 滑动窗口]
categories: LeetCode题解
cover: https://raw.githubusercontent.com/wsylab/wsyPicGo/main/img/photo-1510307853572-cd978ae81304
description: LeetCode-30「串联所有单词的子串」的思考与解答
---
### [LeetCode-30 串联所有单词的子串](https://leetcode.cn/problems/substring-with-concatenation-of-all-words/)

### Solution 1
本题是[LeetCode-438 找到字符串中所有字母异位词](https://leetcode.cn/problems/find-all-anagrams-in-a-string/)的进阶版, 把字母变成了单词. 在[LeetCode-438](https://leetcode.cn/problems/find-all-anagrams-in-a-string/)中的滑动窗口做法仍然可用, 具体实现只需要修改一部分. 我们把窗口滑动的单位改为单词的长度 $n$, 分类讨论起始位置 $0, 1,..., n - 1$ 即可.
代码如下:
```C++
class Solution {
public:
    vector<int> findSubstring(string s, vector<string>& words) {
        vector<int> ans;
        int n = words[0].size();
        // 创建两个哈希表, need存储目标字符串的元素构成, window存储当前窗口字符串的元素构成
        unordered_map<string, int> need;
        for (string word: words) {
            need[word]++;
        }
        for (int i = 0; i < n; i++) { // 分别考虑从0, 1,..., n - 1开头的字符串 以n为单位进行移动
            unordered_map<string, int> window;
            int left = 0, right = 0; //[left * n + i, (right - 1) * n + i]
            int valid = 0;
            while (right * n + i < s.size()) {
                string wordin = s.substr(right * n + i, n);
                right++; // 扩大窗口
                if (need.count(wordin)) {
                    window[wordin]++;
                    if (window[wordin] == need[wordin]) {
                        valid++; // 数据更新
                    }
                }
                while (right - left > words.size() && valid == need.size()) {
                    string wordout = s.substr(left * n + i, n);
                    left++; // 缩小窗口
                    if (need.count(wordout)) {
                        if (window[wordout] == need[wordout]) {
                            valid--; // 数据更新
                        }
                        window[wordout]--;

                    } 
                }
                // 判断一个合法的窗口是否是我们需要的
                if (valid == need.size()) {
                    ans.push_back(left * n + i);
                }
            }
        }
        return ans;
    }
};
```