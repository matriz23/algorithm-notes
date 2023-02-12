---
title: LeetCode-2250 统计包含每个点的矩形数目
date: 2022-05-03 19:54:00
updated: 2022-05-03 23:33:00
tags: [计算机, 算法, LeetCode, 二分搜索, C++, 树状数组]
categories: LeetCode题解
cover: https://raw.githubusercontent.com/wsylab/wsyPicGo/main/img/photo-1469719847081-4757697d117a
description: LeetCode-2250 「统计包含每个点的矩形数目」的思考与解答
---
### [LeetCode-2250 统计包含每个点的矩形数目](https://leetcode-cn.com/problems/count-number-of-rectangles-containing-each-point/)
![](https://raw.githubusercontent.com/wsylab/wsyPicGo/main/img/20220503200118.png)
正如上图所示, 本题数据量较大...$O(n^2)$的暴力算法完全过不了, 必须优化.
### Solution 1
从 $1\leq h_i, y_j\leq 100$ 可以想到本题的第一个优化方向, $y$ 范围不大, 可以单独为每个 $y$ 建立一个数组 $logs[y]$ 用于存放纵坐标为 $y$ 的矩形顶点的横坐标.对每个点 $(px, py)$, 依次遍历 $logs[py],...,logs[100]$ , 统计不小于 $px$ 的元素个数, 累加存入答案数组中即可.
但这次优化后依然会超时, 原因是统计 $logs[py],...,logs[100]$ 不小于 $px$ 的元素个数效率过低. 基于这一缺点我们再次优化, 将 $logs[1],...,logs[100]$ 排序, 再利用二分查找得到不小于 $px$ 的元素个数.
代码如下:
```C++
class Solution {
public:
    vector<int> countRectangles(vector<vector<int>>& rectangles, vector<vector<int>>& points) {
        vector<int> ans;
        int rN = rectangles.size();
        int pN = points.size(); 
        vector<vector<int>> logs(101, vector<int>(0)); //记录每个y对应的x
        for (int i = 0; i < rN; i++) {
            logs[rectangles[i][1]].push_back(rectangles[i][0]);
        }
        for (int i = 1; i <= 100; i++) {
            sort(logs[i].begin(), logs[i].end());
        }
        for (int i = 0; i < pN; i++) {
            int px = points[i][0];
            int py = points[i][1];
            int res = 0;
            for (int j = py; j <= 100; j++) {
                if (logs[j].size() != 0) {
                    int left = 0;
                    int right = logs[j].size();
                    while(left < right) {
                        int mid = left + (right - left) / 2;
                        if (logs[j][mid] == points[i][0]) {
                            right = mid;
                        }
                        else if(logs[j][mid] < points[i][0]) {
                            left = mid + 1;
                        }
                        else if(logs[j][mid] > points[i][0]) {
                            right = mid;
                        }
                      }                   
                    }
                    res = res + logs[j].size() - left;
                }
            } 
            ans.push_back(res);
        }
        return ans;
    }
};
```

### Solution 2
思考本题的目标:对每个点, 统计横纵坐标都不小于它的矩阵顶点个数. 这是一个**二维偏序问题**, 我们可以利用树状数组来解决.对所有的点(包含矩阵顶点)按 $x$ 排序,再依照此顺序进行如下操作:
- 是否为矩阵顶点?
是: 不输出前缀和, 压入数组中
否: 输出前缀和, 不压入数组中

这和利用树状数组处理一般二维偏序问题的过程类似, 只不过把两种点区别对待了.
代码如下:
```C++
\\ 树状数组模板
class BIT {
public :
    BIT(int size) : n(size), c(n + 1) {}
    void update(int i, int x) {
        while (i <= n) c[i] += x, i += lowbit(i);
        return ;
    }

    int query(int x) {
        int sum = 0;
        while (x) sum += c[x], x &= (x - 1);
        return sum;
    }

private:
    int n; // MAXN
    vector<int> c;//数组

    //lowbit()函数定义
    int lowbit(int x) {
        return x & (-x);
    }
};


class Solution {
//结构体 定义一个通用的点, 矩阵顶点标记记为-1, 普通点标记记录其原本的下标.
struct normalPoint{
        int x;
        int y;
        int index;
        normalPoint(int x, int y, int index): x(x), y(y), index(index) {}
    };

public:
    static bool cmp(normalPoint p1, normalPoint p2) {
        if (p1.x - p2.x != 0) {
           return p1.x > p2.x; 
        } // x大的排在前面
        if (p1.y - p2.y != 0) {
            return p1.y > p2.y;
        } // x一样, y大的排在前面
        if (p1.index < p2.index) {
            return true;
        } // x, y都一样, 依据题意只能是一个点一个矩阵, 依据树状数组的输出特性把矩阵排到前面
        return false;
    }

    vector<int> countRectangles(vector<vector<int>>& rectangles, vector<vector<int>>& points) {
        int rN = rectangles.size();
        int pN = points.size();
        vector<int> ans(pN);
        vector<normalPoint> np;
        for (int i = 0; i < rN; i++) {
            np.push_back(normalPoint(rectangles[i][0], rectangles[i][1], -1));
        }
        for (int i = 0; i < pN; i++) {
            np.push_back(normalPoint(points[i][0], points[i][1], i));
        }
        sort(np.begin(), np.end(), cmp); //排好序后的数组, 横坐标从大到小, 纵坐标从大到小, 矩阵排在点之前
        BIT tree(105); // 我们维护y坐标固定下x的数量
        for (int i = 0; i < rN + pN; i++) {
            int index = np[i].index;
            if (index < 0) {
                tree.update(101 - np[i].y, 1); // 横坐标不小于目前点的,纵坐标为101 - np[i].y的点数量+1
            }
            else {
                ans[np[i].index] = tree.query(101 - np[i].y); //查询横坐标不小于当前点,且纵坐标不小于当前点的点数量
            }
        }
        return ans;
    }
};
```