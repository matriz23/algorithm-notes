---
title: LeetCode-2251 花期内花的数目
date: 2022-05-04 14:34:00
updated:
tags: [计算机, 算法, LeetCode, C++, STL, 差分数组, 离散化]
categories: LeetCode题解
cover: https://raw.githubusercontent.com/wsylab/wsyPicGo/main/img/photo-1519882189396-71f93cb4714b
description: LeetCode-2251 「花期内花的数目」的思考与解答
---
### [LeetCode-2251 花期内花的数目](https://leetcode.cn/problems/number-of-flowers-in-full-bloom/)
### Solution 1
本题本质上是求一个点被多少区间覆盖的问题, 或者理解成维护一个数组, 这个数组需要频繁地把某个区间的元素进行增减.
常规的思想是利用**差分数组**进行维护, 考虑原数组 $nums$ ,我们建立一个数组 $diff$ ,它满足 $diff[i] = nums[i] - nums[i - 1]$(便于形式的统一, 我们认为 $nums[-1] = 0$ ) ,这样将区间 $[a,b]$ 的元素全部加上 $k$ 的操作就变成了 $diff[a] = diff[a] + k,diff[b + 1] = diff[b + 1] -k$ , 要得到原数组的某个元素 $nums[j]$ , 只需要把 $diff[0], ..., diff[j]$ 累加即可.
回到本题, 由于数据量的问题, 建立差分数组并依次求来访时间点的花数量会超时. 因此我们需要优化算法.
#### 优化一: 重复计算
首先考虑是否有重复计算的部分, 很明显, 对差分数组求原数组的一个元素相当于求差分数组的前缀和, 而在计算前缀和数组时, 我们利用一个 $Sum$ 按下标依次累加就可以得到所有前缀和. 因此, 根据游客来访的先后排序, 可以避免相当一部分的重复计算. 由于最后输出答案时需要按初始顺序输出, 我们必须在排序后仍能知道原始的顺序. 在这方面我定义了一个结构体:
```C++
struct visitTime{
    int time;
    int index;
    visitTime(int time, int index): time(time), index(index){}
};
```
把结构体 $visitTime$ 存入数组中再排序.
但有一种更简洁的实现方法, 建立一个内容为 $0,1,...,n - 1$的数组, 将其排序方法定义为与结构体类似的即可, 大致如下: 
```C++
iota(id.begin(), id.end(), 0);
sort(id.begin(), id.end(), [&](int i, int j) { return persons[i] < persons[j]; });
``` 
这样也得到了一个按照来访先后排序的序号数组, 累加时按照这一数组顺序即可.

#### 优化二: 无效计算
![](https://raw.githubusercontent.com/wsylab/wsyPicGo/main/img/20220504145533.png)
从数据范围中可以看出, 时间轴很长, 但时间轴中真正用到的点并不多. 如果定义普通的差分数组 $vector<int>diff(1000000001,0)$ , 我们在累加答案时大部分时间都花费在了 $+0$ ,上, 这无疑是超时的一大原因. 为了解决这个问题, 需要**离散化**.
对于用到的(有变化的) $diff[k]$ ,我们才需要考虑它. 这一部分用数据结构 $map$ 实现, 大致如下:
```C++
map<int, int> diff;
for (int i = 0; i < flowers.size(); i++) {
    diff[flowers[i][0]]++;
    diff[flowers[i][1]+1]--;
}
```
在从小到大遍历时, 用 $auto\ it = diff.begin()$ 遍历即可.

#### 代码实现
使用结构体存储的代码如下:
```C++
class Solution {
struct visitTime{
    int time;
    int index;
    visitTime(int time, int index): time(time), index(index){}
};

public:
    static bool cmp(visitTime vt1, visitTime vt2) {
        return vt1.time < vt2.time;
    }

    vector<int> fullBloomFlowers(vector<vector<int>>& flowers, vector<int>& persons) {
        
        map<int, int> diff;
        for (int i = 0; i < flowers.size(); i++) {
            diff[flowers[i][0]]++;
            diff[flowers[i][1]+1]--;
        }
        int pN = persons.size();
        vector<int> ans(pN);
        vector<visitTime> vt;
        for (int i = 0; i < pN; i++) {
            vt.push_back(visitTime(persons[i], i));
        }
        sort(vt.begin(), vt.end(), cmp);
        int nowSum = 0;
        auto it = diff.begin();
        for (int i = 0; i < pN; i++) {
            while (it != diff.end() && it->first <= vt[i].time){
                nowSum = nowSum + it->second;
                it++;
            }
            ans[vt[i].index] = nowSum;
        }
        return ans;
    }
};
```