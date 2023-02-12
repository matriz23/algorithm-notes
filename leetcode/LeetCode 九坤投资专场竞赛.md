---
title: LeetCode 九坤投资专场竞赛
date: 2022-08-23 10:45:00
updated:
tags: [计算机, 算法, LeetCode, C++, Python, 哈希, 搜索, 概率, 动态规划]
categories: LeetCode题解
cover: https://raw.githubusercontent.com/wsylab/wsyPicGo/main/img/photo-1568234928966-359c35dd8327.avif
description: LeetCode「九坤投资专场竞赛」的思考与解答
---
# [LeetCode 九坤投资专场竞赛](https://leetcode.cn/contest/ubiquant2022/)

## Problem 1 可以读通讯稿的组数
### 题目链接
[九坤-01. 可以读通讯稿的组数](https://leetcode.cn/contest/ubiquant2022/problems/xdxykd/)
### Solution 1
如果两个编号 $a, b$ 满足条件 $a + reverse(b) == reverse(a) + b$ , 那么有 $a - reverse(a) == b - reverse(b)$ , 把对称的二元关系转化成一元的性质, 这样用哈希计数即可.
代码如下:
```C++
#define ll long long
const ll MOD = 1e9 + 7;
class Solution {
public:
    int reverse(int num) {
        int res = 0;
        int temp = 0;
        while (num != 0) {
            temp = num % 10;
            res = res * 10 + temp;
            num /= 10;
        }
        return res;
    }
    
    int numberOfPairs(vector<int>& nums) {
        int ans = 0;
        unordered_map<int, ll> book;
        for (int num: nums) {
            book[num - reverse(num)]++;
        }
        for (auto it: book) {
            ans = (ans + (it.second - 1) * (it.second) / 2) % MOD;
        }
        return ans;
    }
};
```

## Problem 2 池塘计数
### 题目链接 
[九坤-02. 池塘计数](https://leetcode.cn/contest/ubiquant2022/problems/3PHTGp/)
### Solution 1
统计池塘数量, 我们用沉岛策略避免重复计算.
代码如下:
```C++
const int dirs[8][2] = {{-1, -1}, {-1, 0}, {-1, 1}, {0, -1}, {0, 1}, {1, -1}, {1, 0}, {1, 1}};
class Solution {
public:
    int m, n;
    void visit(vector<string>& field, int i, int j) {
        field[i][j] = '.'; // 沉岛, 避免重复搜索
        for (auto dir: dirs) {
            int x = i + dir[0];
            int y = j + dir[1];
            if (x >= 0 && x < m && y >= 0 && y < n && field[x][y] == 'W') {
                visit(field, x, y);
            }
        }
        return;
    }
    
    int lakeCount(vector<string>& field) {
        m = field.size();
        n = field[0].size();
        int ans = 0;
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (field[i][j] == 'W') {
                    ans++;
                    visit(field, i, j);
                }
            }
        }
        return ans;
    }
};
```

## Problem 3 数字默契考验
### 题目链接
[九坤-03. 数字默契考验](https://leetcode.cn/contest/ubiquant2022/problems/uGuf0v/)
### Solution 1
经过若干次 $\times 2$ 和 $\times 3$ 的操作后能够得到相同的数, 说明 $numbers$ 数组中每个元素除了 $2$ 和 $3$ 以外的因子是完全相同的. 我们统计各个元素 $2$ 和 $3$ 的幂次与最高幂次的差之和即可.
代码如下:
```C++
#define ll long long
class Solution {
public:
    const int TWO = 1 << 30, THREE = 1162261467;
    int minOperations(vector<int>& numbers) {
        int n = numbers.size();
        vector<int> part_2(n, 0), part_3(n, 0);
        int base = numbers[0] / __gcd(numbers[0], TWO) / __gcd(numbers[0], THREE);
        int max_2 = 0, max_3 = 0;
        int ans = 0;
        for (int i = 0; i < n; i++) {
            part_2[i] = __gcd(numbers[i], TWO);
            part_3[i] = __gcd(numbers[i], THREE);
            if (numbers[i] / part_2[i] / part_3[i] != base) {
                return -1;
            } 
            int cnt_2 = 0, cnt_3 = 0;
            while (part_2[i] % 2 == 0) {
                part_2[i] /= 2;
                cnt_2++;
            }
            while (part_3[i] % 3 == 0) {
                part_3[i] /= 3;
                cnt_3++;
            }
            max_2 = max(max_2, cnt_2);
            max_3 = max(max_3, cnt_3);
            ans -= cnt_2 + cnt_3;
        }
        ans += (max_2 + max_3) * n;
        return ans;
    }
};
``` 

## Problem 4 筹码游戏
### 题目链接
[九坤-04. 筹码游戏](https://leetcode.cn/contest/ubiquant2022/problems/I3Gm2h/)
### Solution 1
由于筹码的地位对称, 我们只需要考虑筹码的数量而不必纠结于具体的分配. 用 $k$ 个单调不减的数字代表当前筹码的状态 $state$ . 考虑到达终点 $nums$ 的状态转移. 每次抽取筹码共有 $k$ 种可能, 每种使得 $state[i] + 1$ 得到一种新的 $state_i$ . 假设有 $m$ 个状态可以到达最终状态, 则有如下状态转移方程:
$$
E(state) = \sum_{t = 1}^{m}E(state_{j_t}) \times \frac{1}{kind} + (1 - \frac{m}{kind})\times E(state) + 1
$$
即
$$
E(state) = \frac{\sum_{t = 1}^{m}E(state_{j_t}) + kind}{m}
$$
使用深度优先搜索来完成递推过程, 利用记忆化搜索减少计算量.
代码如下:
```Python 3
class Solution:
    def chipGame(self, nums: List[int], kind: int) -> float:
        nums = [0] * (kind - len(nums)) + sorted(nums)
        @cache
        def getRes(state):
            state = list(state)
            if state == nums: return 0
            sum_E = 0
            sum_p = 0
            i = 0
            
            for i in range(kind):
                cnt = 1
                state[i] += 1
                new_state = sorted(state)
                for x, y in zip(new_state, nums):
                    if x > y:
                        break;
                else:
                    sum_E += getRes(tuple(new_state)) 
                    sum_p += 1
                state[i] -= 1
            return (sum_E + kind) / sum_p
        return getRes(tuple([0] * kind))
```