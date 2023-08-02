# [NOIP2003 普及组] 栈

## 题目背景

栈是计算机中经典的数据结构，简单的说，栈就是限制在一端进行插入删除操作的线性表。

栈有两种最重要的操作，即 pop（从栈顶弹出一个元素）和 push（将一个元素进栈）。

栈的重要性不言自明，任何一门数据结构的课程都会介绍栈。宁宁同学在复习栈的基本概念时，想到了一个书上没有讲过的问题，而他自己无法给出答案，所以需要你的帮忙。

## 题目描述

![](https://cdn.luogu.com.cn/upload/image_hosting/iwqdpb7b.png)

宁宁考虑的是这样一个问题：一个操作数序列，$1,2,\ldots ,n$（图示为 1 到 3 的情况），栈 A 的深度大于 $n$。

现在可以进行两种操作，

1. 将一个数，从操作数序列的头端移到栈的头端（对应数据结构栈的 push 操作）
2. 将一个数，从栈的头端移到输出序列的尾端（对应数据结构栈的 pop 操作）

使用这两种操作，由一个操作数序列就可以得到一系列的输出序列，下图所示为由 `1 2 3` 生成序列 `2 3 1` 的过程。

![](https://cdn.luogu.com.cn/upload/image_hosting/mj7hxtsu.png)

（原始状态如上图所示）

你的程序将对给定的 $n$，计算并输出由操作数序列 $1,2,\ldots,n$ 经过操作可能得到的输出序列的总数。

## 输入格式

输入文件只含一个整数 $n$（$1 \leq n \leq 18$）。

## 输出格式

输出文件只有一行，即可能输出序列的总数目。

## 样例 #1

### 样例输入 #1

```
3
```

### 样例输出 #1

```
5
```

## 提示

**【题目来源】**

NOIP 2003 普及组第三题

## AC代码

```cpp
#include <bits/stdc++.h>
#define ll long long
using namespace std;

const int N = 20;

ll dp[N][N];

ll dfs(ll i, ll j) {//i表示当前有几个数没有放进去,j表示当前栈中有几个数
  if (i == 0) {
    dp[i][j] = 1;
    return 1;
  }
  if (dp[i][j]) return dp[i][j];
  if (j == 0) {
    dp[i][j] = dfs(i - 1, j + 1);
  } else {
    dp[i][j] = dfs(i - 1, j + 1) + dfs(i, j - 1);
  }
  return dp[i][j];
}

signed main() {
  cin.tie(nullptr)->sync_with_stdio(false);
  ll n;
  cin >> n;

  cout << dfs(n, 0);

  return (0 ^ 0);
}
```

# 硬币问题

## 题目描述

今有面值为 1、5、11 元的硬币各无限枚。

想要凑出 $n$ 元，问需要的最少硬币数量。

## 输入格式

仅一行，一个正整数 $n$。

## 输出格式

仅一行，一个正整数，表示需要的硬币个数。

## 样例 #1

### 样例输入 #1

```
15
```

### 样例输出 #1

```
3
```

## 样例 #2

### 样例输入 #2

```
12
```

### 样例输出 #2

```
2
```

## 提示

#### 样例解释

对于样例数据 1，最佳方案是 $15=5+5+5$，使用到 3 枚硬币。

对于样例数据 2，最佳方案是 $12=11 + 1$，使用到 2 枚硬币。

#### 数据规模与约定

对于 $100\%$ 的数据，保证 $n\leq 10^6$。

## AC代码

```cpp
#include <bits/stdc++.h>
#define ll long long
using namespace std;

const int N = 1e6 + 10;

ll dp[N];

signed main() {
  cin.tie(nullptr)->sync_with_stdio(false);
  ll n;
  cin >> n;

  dp[0] = 0;
  dp[1] = 1;
  dp[2] = 2;
  dp[3] = 3;
  dp[4] = 4;
  dp[5] = 1;
  dp[6] = 2;
  dp[7] = 3;
  dp[8] = 4;
  dp[9] = 5;
  dp[10] = 2;
  dp[11] = 1;

  for (ll i = 12; i <= n; i++) {
    dp[i] = min({dp[i - 1], dp[i - 5], dp[i - 11]}) + 1;
  }

  cout << dp[n];

  return (0 ^ 0);
}
```

