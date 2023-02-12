---
title: LeetCode-854 相似度为 K 的字符串
date: 2022-09-13 16:44:00
updated:
tags: [计算机, 算法, LeetCode, C++, 贪心]
categories: LeetCode题解
cover: https://raw.githubusercontent.com/wsylab/wsyPicGo/main/img/photo-1661601339532-ba01cbb2daf8.avif
description: LeetCode-854「相似度为 K 的字符串」的思考与解答
---
### [LeetCode-854 相似度为 K 的字符串](https://leetcode.cn/problems/k-similar-strings/)

### Solution 1
一次交换至少应该匹配一个字符, 如果能匹配两个字符就一定是最优的. 基于这种贪心思想我们用广度优先搜索来寻找最小的 $k$ .
代码如下:
```C++
class Solution {
public:
    string swap(int i, int j, string s) {
        char temp = s[i];
        s[i] = s[j];
        s[j] = temp;
        return s;
    }

    int kSimilarity(string s1, string s2) {
        int n = s1.size();
        queue<string> q;
        q.push(s1);
        if (s1 == s2) {
            return 0;
        }
        int ans = 0;
        while (!q.empty()) {
            ans++;
            int k = q.size();
            while (k--) {
                string s = q.front();
                q.pop();
                int idx = -1;
                char target;
                for (int i = ans - 1; i < n; i++) {
                    if (s[i] != s2[i]) {
                        idx = i;
                        target = s2[i];
                        break;
                    }
                }
                for (int i = idx + 1; i < n; i++) {
                    if (s[i] == target) {
                        string res = swap(idx, i, s);
                        if (res == s2) {
                            return ans;
                        }
                        q.push(res);
                        if (s[idx] == s2[i]) {
                            break;
                        }
                    }
                }
            }
        }
        return 0;
    }
};
```