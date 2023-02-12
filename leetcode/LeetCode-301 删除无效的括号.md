---
title: LeetCode-301 删除无效的括号 
date: 2022-09-05 10:51:00
updated:
tags: [计算机, 算法, LeetCode, C++, 搜索]
categories: LeetCode题解
cover: https://raw.githubusercontent.com/wsylab/wsyPicGo/main/img/photo-1451337516015-6b6e9a44a8a3
description: LeetCode-301「删除无效的括号」的思考与解答
---
### [LeetCode-301 删除无效的括号](https://leetcode.cn/problems/remove-invalid-parentheses/)

### Solution 1
要求返回所有的可能结果, 同时要求删除步数最小, 所以使用深度优先搜索. 具体实现时, 模拟每一步可能的删除情况, 同时用一个 $set$ 去重.
代码如下:
```C++
class Solution {
public:
    bool isValid(string s) {
        stack<char> st;
        for (auto ch: s) {
            if (ch == '(') {
                st.push(ch);
            }
            else if (ch == ')') {
                if (st.empty()) {
                    return false;
                }
                st.pop();
            }
        }
        return st.empty();
    }

    vector<string> removeInvalidParentheses(string s) {
        set<string> book;
        queue<string> q;
        
        vector<string> ans; 
        book.insert(s);
        q.push(s);
        if (isValid(s)) {
            ans.push_back(s);
            return ans;
        }
        while (!q.empty()) {
            int sz = q.size();
            while (sz--) {
                string t = q.front();
                cout<<t<<endl;
                q.pop();
                book.erase(t);
                for (int i = 0; i < t.size(); i++) {
                    if (t[i] != '(' && t[i] != ')') {
                        continue;
                    }
                    string ns = "";
                    for (int j = 0; j < t.size(); j++) {
                        if (j != i) {
                            ns += t[j];
                        }
                    }
                    if (!book.count(ns)) {
                        book.insert(ns);
                        q.push(ns);
                        if (isValid(ns)) {
                            ans.push_back(ns);
                        }
                    }                    
                }
                
            }
            if (ans.size() != 0) {
                return ans;
            }
        }
        return ans;
    }
};
```