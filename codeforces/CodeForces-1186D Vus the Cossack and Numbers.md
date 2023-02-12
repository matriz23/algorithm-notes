---
title: CodeForces-1186D Vus the Cossack and Numbers 
date: 2022-08-03 23:00:00
updated:
tags: [计算机, 算法, CodeForces, C++, 浮点数, 构造]
categories: CodeForces题解
cover: https://raw.githubusercontent.com/wsylab/wsyPicGo/main/img/photo-1508695078267-96141d2cfbbd
description: CodeForces-1186D「Vus the Cossack and Numbers」的思考与解答
---
### [CodeForces-1186D Vus the Cossack and Numbers]()
### 题目大意
给定 $n$ 的和为 $0$ 的浮点数数组 $a_1, a_2, ..., a_n$ , 构造出一个和为 $0$ 的整数数组 $b_1, b_2, ..., b_n$ , 满足 $b_i = \lfloor a_i\rfloor$ 或 $\lceil a_i\rceil$ .
### Solution 1
不妨设浮点数数组不包含恰好为整数的数. 假设合题意的构造中有 $x$ 个数向上取整 (设为前 $x$ 个数) , 则有 $0 = \sum_{i = 1}^{n} b_i = \sum_{i = 1}^{x} (1 + \lfloor a_i\rfloor) + \sum_{i = x + 1}^{n} \lfloor a_i\rfloor = x + \sum_{i = 1}^{n}(\lfloor a_i\rfloor - a_i)$ , 故 $x = \sum_{i = 1}^{n}(a_i - \lfloor a_i \rfloor)$ . 计算出 $x$ 后, 构造 $b$ 即可.
需要注意一些精度方面的问题.
代码如下:
```C++
#include <bits/stdc++.h>
using namespace std;
int main() {
    int n;
    cin>>n;
    vector<double> a(n, 0);
    vector<int> b(n, 0);
    double sum = 0;
    for (int i = 0; i < n; i++) {
        cin>>a[i];
        sum += a[i] - floor(a[i]);
    }
    int int_sum;
    if (sum - floor(sum) < 1e-6) {
        int_sum = floor(sum);
    }
    else {
        int_sum = ceil(sum);
    }
    for (int i = 0, cnt = 0; i < n; i++) {
        if (a[i] - floor(a[i]) < 1e-6) {
            b[i] = int(a[i]);
        }
        else {
            b[i] = cnt < int_sum? ceil(a[i]): floor(a[i]);
            cnt++;
        }
        cout<<b[i]<<endl;
    }
    return 0;
}
``` 

### Solution 2
在 Solution 1 中, 我们通过计算得到了上取整的元素个数. 一种更本质的想法是, $\sum_{i = 1}^{n}\lfloor a_i\rfloor \leq \sum_{i = 1}^{n} a_i = 0, \sum_{i = 1}^{n}\lceil a_i\rceil \geq \sum_{i = 1}^{n}a_i = 0$ .初始令 $b_i = \lfloor a_i\rfloor$ , 不断地让 $b$ 中的不为整数的元素 $+ 1$ . 类似介值定理, 一定有一刻整个数组和恰为 $0$ , 这样就得到了一个合法的构造.
代码如下:
```C++
#include <bits/stdc++.h>
using namespace std;
int main() {
    int n;
    cin>>n;
    vector<double> a(n, 0);
    vector<int> b(n, 0);
    int sum = 0;
    for (int i = 0; i < n; i++) {
        cin>>a[i];
        b[i] = floor(a[i]);
        sum += b[i];
    }
    for (int i = 0; i < n; i++) {
        if (sum < 0 && a[i] - b[i] > 1e-6) {
            b[i]++;
            sum++;
        }
        cout<<b[i]<<endl;
    }
    return 0;
}
```