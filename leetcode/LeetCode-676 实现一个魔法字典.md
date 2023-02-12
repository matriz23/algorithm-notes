---
title: LeetCode-676 实现一个魔法字典
date: 2022-07-11 16:54:00
updated:
tags: [计算机, 算法, LeetCode, C++, 字典树, 数据结构]
categories: LeetCode题解
cover: https://raw.githubusercontent.com/wsylab/wsyPicGo/main/img/photo-1453224424525-aeb893f2f1ca
description: LeetCode-676「实现一个魔法字典」的思考与解答
---
### [LeetCode-676 实现一个魔法字典](https://leetcode.cn/problems/implement-magic-dictionary/)

### Solution 1
本题可以暴力求解, 也可以通过字典树优化求解. 这一题最值得注意的是编写递归函数时的逻辑. 递归寻找一个 `word` 时, 在某一个节点处, 可以选择继续下去, 也可以选择变化, 而不是必须二选一; 在选择变化的过程中, 也是遇到 `true` 才返回, 遇到 `false` 不需要返回, 理顺逻辑体系才能写出 bug free 的代码.
代码如下:
```C++
struct Trie {
    Trie* children[26];
    bool isEnd; 

    Trie() {
        isEnd = false;
        fill(begin(children), end(children), nullptr);
    }
};

class MagicDictionary {
private:
    Trie* root;

public:
    MagicDictionary() {
        root = new Trie();
    }
    
    void buildDict(vector<string> dictionary) {
        for (auto word: dictionary) {
            Trie* cur = root;
            for (auto ch: word) {
                ch -= 'a';
                if (cur->children[ch] == nullptr) {
                    cur->children[ch] = new Trie();
                }
                cur = cur->children[ch];
            }
            cur->isEnd = true;
        }
    }
    
    bool searchModifiedWord(Trie* node, string word, int len, bool isModified) {
        if (len == word.size()) {
            cout<<word<<" dot 1: "<<(isModified && node->isEnd)<<endl; 
            return isModified && node->isEnd;
        }
        char ch = word[len];
        ch -= 'a'; 
        if (node->children[ch] != nullptr) {
            if (searchModifiedWord(node->children[ch], word, len + 1, isModified)){
                return true;
            }
        } 
        if (!isModified) {
            for (int i = 0; i < 26; i++) {
                if (node->children[i] != nullptr && i != ch) {
                    if (searchModifiedWord(node->children[i], word, len + 1, true)) {
                        return true;
                    }
                }
            } 
        }
        return false;
    }

    bool search(string searchWord) {
        return searchModifiedWord(root, searchWord, 0, false);
    }
};
```