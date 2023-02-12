---
title: CodeForces-1426F Number of Subsequences 
date: 2022-08-04 11:27:00
updated:
tags: [计算机, 算法, CodeForces, C++, 动态规划, 字符串]
categories: CodeForces题解
cover: https://raw.githubusercontent.com/wsylab/wsyPicGo/main/img/photo-1501707305551-9b2adda5e527
description: CodeForces-1426F「Number of Subsequences」的思考与解答
---
### [CodeForces-1426F Number of Subsequences](http://codeforces.com/problemset/problem/1426/F)
### 题目大意
给定一个长度为 $n$ 且由 $a,b,c,?$ 四种字符构成的字符串 $s$ . 每个 $?$ 都可能是 $a, b, c$ 三种字符中的任意一种. 求 $s$ 的所有变化中子序列 $abc$ (不要求连续) 出现的次数.
### Solution 1
要求出 $s$ 所有变化中子序列 $abc$ 的出现次数, 显然一个 $abc$ 匹配中, 需要的 $?$ 越多, 它在所有变化中出现得次数就越少. 我们根据这一点, 根据 $?$ 个数分别统计 $abc$ 的个数, 用 $cnt$ 数组记录. 
怎么快速地进行统计呢? 考虑从左向右遍历, 遇到 $c$ 或 $?$ 时, 才可能产生新的 $abc$ , 对于一个 $c$ 或 $?$ , 我们需要知道, 在它前面一共出现了多少次 $ab, ?b, a?, ??$ . 以 $ab$ 为例, 这一部分实质上是对 $b$ 前面 $a$ 的个数的累加 (类似前缀和的前缀和) . 第一次遍历, 我们记录每个元素前面 $a$ 的个数和 $?$ 的个数. 第二次遍历, 遇到 $b$ , 我们就累加 $a$ 和 $?$ 的个数, 遇到 $c$ 我们就计算匹配子序列的个数, 遇到 $?$ 的情况特殊一点, 我们先把 $?$ 视作 $c$ , 计算匹配子序列的个数, 再把 $?$ 视作 $b$ , 计算匹配前缀的个数, 注意这两步顺序是不可以颠倒的.
得到 $cnt$ 数组后, 我们就能计算出 $ans$ 了.
代码如下: 
```C++
#include <bits/stdc++.h>
using namespace std;
#define ll long long
const ll MOD = 1e9 + 7;

ll quickpow(ll x, ll n){
    ll res = x;
    ll ans = 1;
    while (n) {
        if(n & 1){
            ans = (ans * res) % MOD;
        }
        res = (res * res) % MOD;
        n = n>>1;
    }
    return ans;
}

int main() {
    int n;
    string s;
    cin>>n>>s;
    vector<ll> pre_a(n + 1, 0);
    vector<ll> pre_d(n + 1, 0);
    for (int i = 1; i <= n; i++) {
        pre_a[i] = (pre_a[i - 1] + (s[i - 1] == 'a')) % MOD; // 记录每个字符前面有多少个 a
        pre_d[i] = (pre_d[i - 1] + (s[i - 1] == '?')) % MOD; // 记录每个字符前面有多少个 ?
    }
    ll cnt_ab = 0, cnt_ad = 0, cnt_db = 0, cnt_dd = 0; // 匹配前缀的累加数量
    ll ans = 0;
    vector<ll> cnt(4, 0);
    for (int i = 0; i < n; i++) {
        if (s[i] == 'b') {
            cnt_ab = (cnt_ab + pre_a[i]) % MOD;
            cnt_db = (cnt_db + pre_d[i]) % MOD;
        }
        else if (s[i] == '?') {
            cnt[1] = (cnt[1] + cnt_ab) % MOD;
            cnt[2] = (cnt[2] + cnt_db + cnt_ad) % MOD;
            cnt[3] = (cnt[3] + cnt_dd) % MOD;
            cnt_ad = (cnt_ad + pre_a[i]) % MOD;
            cnt_dd = (cnt_dd + pre_d[i]) % MOD;
        }
        else if (s[i] == 'c') {
            cnt[0] = (cnt[0] + cnt_ab) % MOD;
            cnt[1] = (cnt[1] + cnt_db + cnt_ad) % MOD;
            cnt[2] = (cnt[2] + cnt_dd) % MOD;
        }
    }
    for (int i = 0; i < 4; i++) {
        if (i <= pre_d[n]) {
            ans = (ans + cnt[i] * quickpow(3, pre_d[n] - i)) % MOD;
        }
    }
    cout<<ans<<endl;
    return 0;
}
```

### Solution 2
从匹配子串前缀的角度考虑, 对于 $abc$ , 匹配进度为 $a, ab, abc$ , 不妨用 $f[i][j]$ 记录前 $i + 1$ 个字符中, 第 $j$ 中匹配模式的数量之和. 设一共有 $m$ 个 $?$ , 则可能的字符串一共有 $3^m$ 个. 如果第 $i$ 个字符是 $a$ , 则在 $f[i - 1][j]$ 的基础上又增加了 $3^m$ 个 $a$ , 写成状态转移方程如下:
$$
f[i][0] = f[i - 1][0] + 3^m\\
f[i][1] = f[i - 1][1]\\
f[i][2] = f[i - 1][2]
$$
对于 $b$, 则有:
$$
f[i][0] = f[i - 1][0]\\
f[i][1] = f[i - 1][0] + f[i - 1][1]\\
f[i][2] = f[i - 1][2]
$$
对于 $c$ , 同样有:
$$
f[i][0] = f[i - 1][0]\\
f[i][1] = f[i - 1][1]\\
f[i][2] = f[i - 1][1] + f[i - 1][2]
$$
下面我们来考虑 $?$ 带来的状态转移. 在 $3^m$ 个子串中, $?$ 为 $a, b, c$ 的情况各为 $\frac{1}{3}$ . 因此对上述三种情况作平均即可. 有如下状态转移方程:
$$
f[i][0] = f[i - 1][0] + 3^{m - 1}\\
f[i][1] = \frac{f[i - 1][0]}{3} + f[i - 1][1]\\
f[i][2] = \frac{f[i - 1][1]}{3} + f[i - 1][2]
$$
最终所求答案即为 $f[n - 1][2]$ .
代码如下:
```C++
#include <bits/stdc++.h>
using namespace std;
#define ll long long
const ll MOD = 1e9 + 7;
const ll INV3 = 333333336; // INV3 为 3 关于 1e9 + 7 的逆元

ll quickpow(ll x, ll n){ 
    ll res = x;
    ll ans = 1;
    while (n) {
        if(n & 1){
            ans = (ans * res) % MOD;
        }
        res = (res * res) % MOD;
        n = n>>1;
    }
    return ans;
}

int main() {
    int n;
    string s;
    cin>>n>>s;
    ll m = 0;
    for (char ch: s) {
        if (ch == '?') {
            m++;
        }
    }
    m = quickpow(3, m); // 计算 3 ^ m 
    vector<vector<ll>> dp(n, vector<ll>(3));
    dp[0][0] = (s[0] == 'a') * m + (s[0] == '?') * m * INV3 % MOD;
    for (int i = 1; i < n; i++) {
        if (s[i] != '?') {
            dp[i][0] = (s[i] == 'a')? (dp[i - 1][0] + m) % MOD: dp[i - 1][0];
            dp[i][1] = (s[i] == 'b')? (dp[i - 1][0] + dp[i - 1][1]) % MOD: dp[i - 1][1];
            dp[i][2] = (s[i] == 'c')? (dp[i - 1][1] + dp[i - 1][2]) % MOD: dp[i - 1][2];
        }
        else {
            dp[i][0] = (dp[i - 1][0] + m * INV3) % MOD;
            dp[i][1] = (dp[i - 1][0] * INV3 + dp[i - 1][1]) % MOD;
            dp[i][2] = (dp[i - 1][1] * INV3 + dp[i - 1][2]) % MOD;
        }
    }
    cout<<dp[n - 1][2]<<endl;
    return 0;
}
```

### Solution 3
一种清晰的分割方式是根据中心点分割. 对于每个 $b$ 或 $?$ , 统计其左侧的 $a$ 和 $?$ , 以及右侧的 $c$ 和 $?$ . 注意这种统计方式下, $b$ 和 $?$ 是没有区别的, 两者所在的字符串数量只由左侧和右侧的 $?$ 数量决定 (这一步其实已经把 $?$ 自身给排除了) . 设左侧有 $l_a$ 个 $a$ , $l_d$ 个 $?$ , 右侧有 $r_c$ 个 $c$ , $r_d$ 个 $?$ . 则匹配模式的个数如下所示: 
$$
a*c:\ l_a \times r_c \times 3^{l_d + r_d} \\
?*c:\ l_d \times r_c \times 3^{l_d + r_d - 1} \\
a*?:\ l_a \times r_d \times 3^{l_d + r_d - 1} \\
?*?:\ l_d \times r_d \times 3^{l_d + r_d - 2}
$$
累加即可得到答案.
代码如下:
```C++
#include <bits/stdc++.h>
using namespace std;
#define ll long long
const ll MOD = 1e9 + 7;
const ll INV3 = 333333336;
ll quickpow(ll x, ll n){
    ll res = x;
    ll ans = 1;
    while (n) {
        if(n & 1){
            ans = (ans * res) % MOD;
        }
        res = (res * res) % MOD;
        n = n>>1;
    }
    return ans;
}

int main() {
    int n;
    string s;
    cin>>n>>s;
    vector<ll> l_a(n, 0), l_d(n, 0), r_c(n, 0), r_d(n, 0);
    for (int i = 0; i < n; i++) {
        l_a[i] = i == 0? 0: l_a[i - 1] + (s[i - 1] == 'a');
        l_d[i] = i == 0? 0: l_d[i - 1] + (s[i - 1] == '?');
        r_c[n - 1 - i] = i == 0? 0: r_c[n - i] + (s[n - i] == 'c');
        r_d[n - 1 - i] = i == 0? 0: r_d[n - i] + (s[n - i] == '?');
    }
    ll ans = 0;
    for (int i = 1; i < n - 1; i++) {
        if (s[i] == 'b' || s[i] == '?') {
            ll pow = quickpow(3, l_d[i] + r_d[i]);
            ans = (ans + ((l_a[i] * r_c[i]) % MOD * pow) % MOD) % MOD;
            ans = (ans + ((l_d[i] * r_c[i]) % MOD * pow) % MOD * INV3) % MOD;
            ans = (ans + ((l_a[i] * r_d[i]) % MOD * pow) % MOD * INV3) % MOD;
            ans = (ans + (((l_d[i] * r_d[i]) % MOD * pow) % MOD * INV3) % MOD * INV3) % MOD;
        }
    }
    cout<<ans<<endl;
    return 0;
}
```