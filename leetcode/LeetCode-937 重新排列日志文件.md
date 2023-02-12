---
title: LeetCode-937 重新排列日志文件
date: 2022-05-03 17:33:00
updated:
tags: [计算机, 算法, LeetCode, STL, C++]
categories: LeetCode题解
cover: https://raw.githubusercontent.com/wsylab/wsyPicGo/main/img/photo-1486847484273-512bf6dae1d1
description: LeetCode-937 「重新排列日志文件」的思考与解答
---
### [LeetCode-937 重新排列日志文件](https://leetcode.cn/problems/reorder-data-in-log-files/)
本题是一道自定义排序题, 需要理解题意后设计合适的排序算法.
### Solution 1
 利用C++中sort函数的自定义排序功能来解决这道问题.
#### sort()函数
```C++
// vector<>() vec;
// cmp 为自定义的比较函数C++
// 对 vec 原地排序，将其按cmp定义的从小到大的顺序排列
std::sort(vec.begin(), vec.end(), cmp);
//若未定义cmp, 则以默认方式从小到大排列
```
#### cmp()函数
```C++
// 从大到小:
static bool cmp(int a, int b) {
    return a > b;
// 从小到大:
static bool cmp(int a, int b) {
    return a < b;
// cmp可以理解为:传入两个参数, (a, b)排序是否是正确的, 若是正确的返回true, 否则返回false
// 在这种理解下, 从大到小的cmp函数本质上就是这样:
static bool cmp(int a, int b) {
    if (a > b) {
        return true; // 符合a > b
    }
    return false; // 不符合a > b, 需要交换
```
#### 题目分析
回到本题, 我们利用sort()和自定义cmp()来实现排序功能.
日志共有两种, 字母类和数字类, 同时最后排序完成时字母类需要排在数字类前面, 所以我们考虑分别用两个Vector来存储字母类日志和数字类日志, 对字母类日志排序后将数字类日志插入到字母类Vector的末尾.

#### 代码实现
```C++
class Solution {
public:
    static bool cmp(string s1, string s2) {
        //寻找标识符与日志内容的分界线
        int p1 = s1.find(' ');
        int p2 = s2.find(' ');
        string log1 = s1.substr(p1 + 1);
        string log2 = s2.substr(p2 + 1);
        if (log1 != log2) {
            return log1 < log2;// 日志内容不同时, 对日志内容进行字典序比较, 按字典序从小到大
        }
        return s1 < s2;// 日志内容相同时, 对标识符进行字典序比较, 按字典序从小到大
    }
    
    vector<string> reorderLogFiles(vector<string>& logs) {
        vector<string> letter, number;
        for (int i = 0; i < logs.size(); i++) {
            int p = logs[i].find(' ');
            if (logs[i][p + 1] < 'a') {
                number.push_back(logs[i]);
            }
            else {
                letter.push_back(logs[i]);
            }
        }
        sort(letter.begin(), letter.end(), cmp);// 自定义排序
        letter.insert(letter.end(), number.begin(), number.end());
        return letter;
    }
};
```