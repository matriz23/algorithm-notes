---
title: CodeForces-1442A Extreme Subtraction 
date: 2023-02-10 17:56:00
updated:
tags: [计算机, 算法, CodeForces, C++, 贪心]
categories: CodeForces题解
cover: 
description: CodeForces-1442A「Extreme Subtraction」的思考与解答
---
### [CodeForces-1442A Extreme Subtraction](https://codeforces.com/problemset/problem/1442/A)
### 题目大意
给定数组 $a$ , 每次可以选择它的一段前缀或后缀, 让这些元素 $-1$ . 询问能否将 $a$ 中元素全部变为 $0$ .
### Solution 1
首先, 操作的顺序对结果没有影响, 因此, 可以把操作视作:
- 对前缀进行操作
- 对后缀进行操作

如果只对后缀进行操作就能让数组变为 $0$ , 那么这个数组必须是单调不减的. 
假设经过前缀操作后, $a_i$ 变成了 $0$ , 应当有 $a_{i - 1} \geq a_i$ . 因此, 可以先找到 $a$ 的一个单调不增的最长前缀 $a_0, ... ,a_l$, 显然这个前缀可以变为 $0$ . 类似, 我们可以找到 $a$ 单调不减的最长后缀 $a_r, ..., a_{n - 1}$ . 
此时问题转化为: 对 $a_{l + 1}, ..., a_{r-1}$ 进行一系列前缀操作, 即让它们分别减去 $d_{l+1}, ... ,d_{r - 1}$ , 要求 $a_l \geq d_{l+1} \geq...\geq d_{r - 1}$ , 且 $0 = a_l \leq a_{l+1} - d_{l+1} \leq ...\leq a_{r - 1} - d_{r - 1}\leq a_r$ . 遍历 $a_{l+1}, ..., a_{r - 1}$ , 逐步寻找可行的 $d$ 即可. 这是一个构造性的解法.
代码如下:
```C++
#include <bits/stdc++.h>
using namespace std;

int main() {
	int t;
	cin>>t;
	while (t--) {
		int n;
		cin>>n;
		int a[n];
		for (int i = 0; i < n; i++) {
			cin>>a[i];
		}
		int l;
		for (l = 0; l < n - 1;) {
			if (a[l + 1] <= a[l]) {
				l++;
			}
			else {
				break;
			}
		}
		int r;
		for (r = n - 1; r > 0;) {
			if (a[r - 1] <= a[r]) {
				r--;
			}
			else {
				break;
			}
		}
		if (r - l <= 1) {
			cout<<"YES"<<endl;
			continue;
		}
		bool ans = true;
		int d = a[l];
		a[l] = 0;
		for (int i = l + 1; i < r; i++) {
			d = min(d, min(a[i], a[i] - a[i - 1]));
			a[i] -= d;
			if (a[i] < a[i - 1]) {
				ans = false;
				break;
			}
		}
		if (a[r - 1] > a[r]) {
			ans = false;
		}
		printf("%s\n", ans? "YES": "NO");
	}
	return 0;
}
```

### Solution 2
考虑原数组的差分数组. 如果最终数组元素全为 $0$ , 那么差分数组的元素同样全为 $0$ . 考虑两种操作对差分数组 $d$ 的影响: 
- 对前缀 $a_0, ..., a_i$ 进行操作, 那么 $d_0$ 减 $1$ 且 $d_{i+1}$ 加 $1$ 
- 对后缀 $a_i, ..., a_{n-1}$ 进行操作, 那么 $d_{i}$ 减 $1$ (且 $d_{n}$ 加 $1$ , 不过 $d_{n}$ 与本题无关)

由此, 通过前缀操作可以将 $d$ 中的负数 (首项除外) 变为 $0$ , 通过后缀操作可以将 $d$ 中的正数变为 $0$ . 所以, 当且仅当 $d_0 \geq \sum_{v\in d, v < 0}\vert v\vert$ 时, 可以通过操作使得数组元素全部变为 $0$ .
代码如下: 
```C++
#include <bits/stdc++.h>
using namespace std;
int main() {
    int t;
    cin >> t;
    while (t--) {
        int n;
        cin >> n;
        int a[n];
        for (int i = 0; i < n; i++) {
            cin >> a[i];
        }
        int res = 0;
        for (int i = n - 1; i >= 1; i--) {
            a[i] -= a[i - 1];
            if (a[i] < 0) {
                res -= a[i];
            }
        }
        printf("%s\n", a[0] >= res ? "YES" : "NO");
    }
    return 0;
}
```