---
title: 洛谷 2020 提高组夏令营 模拟赛 1 赛后补题
tags:
  - 树上问题
  - 图论
  - 构造
categories: []
mathjax: true
date: 2020-07-31 16:57:54
description:
---

值得复习
<!--more-->

## A.连接前后缀

40 Pts: 暴力枚举，把每个枚举出来的前后缀加起来丢进一个 `map` 

时间复杂度 $O(nm \log nm)$

80 Pts: 把 40 Pts 的做法改成使用字符串 hash

时间复杂度 $O(nm)$

100 Pts: 观察到这个问题其实和如下问题等价：求出字符串 $p + q$ 有多少种重复的情况，用 $nm$ 减去这个答案的值就是了。

所以我们来关注如何快速求出重复段。

样例 1 里，如果第一个字符串里我们选择了 `ab` 第二个选择了 `a` ，那么观察到如果第一个字符串选择了 `a` 第二个选择了 `ba` 。那么就是一个重复段。

于是我们可以发现，如果第一个与第二个有重复的字符，显然会造成重复段的出现。那么重复段的数量是多少呢？还是从这个样例入手：

1. `aba` 会有 1 个重复：`ab-a` ,`a-ba`
2. `abba` 会有 2 个重复：`a-bba`,`ab-ba`,`a-bba`
3. `abbba` 会有 1 个重复：`ab-bba`,`abb-ba`

这样重复数也就是 4.

当你把样例都推一遍时，你能得出一个式子：

重复段个数 = $\sum_{i = a}^z cnt_A * cnt_B$

其含义是重复段个数为 A 和 B 中重复字母的乘积的和。

最后注意，因为是前缀/后缀，所以第一个/最后一个是必选的，不用统计进去。

代码：

```cpp
const int N = 1000010;
char s[N],p[N];
int n,m,cnt1[N],cnt2[N];
ll ans = 0;
signed main() {
	scanf("%s%s",s,p);
    n = strlen(s),m = strlen(p);
    for (int i = 1;i < n;++i) cnt1[s[i] - 'a' + 1]++;
    for (int i = 0;i < m - 1;++i) cnt2[p[i] - 'a' + 1]++;
    for (int i = 1;i <= 26;++i) {
        ans += 1LL * cnt1[i] * cnt2[i];
    }
    printf("%lld",1LL * n*m-ans);
	return 0;
}
```

## B.合并排列

一个很玄学的题。

首先分析一下题目，它要求的是两个 **排列** 。

说明其必须是从 $1\dots n$ 的一个数列。

所以，如果一个数字在这个数列中没有出现两次，那么这个就无法被拼出来。

排除掉第一种情况，我们就可以把问题转化一下：把一个数列给黑白染色，让黑白部分均为一个 $1\dots n$的排列。也就是说，这个数列有解当且仅当其可以被划分成若干段且满足相关条件。

再看下题目的条件：相当于每次从两个排列中取出较小的那个，那么如果 $a_i > a_{i+1}$ ，显然这两个并不是来自一个排列的。（如果是来自一个排列的显然 $a_{i+1}$ 就先出来了）

所以每一段都是当前的前缀最大值以及后面的一串以及到接下来的下一个前缀最大值。

可能有点抽象，举个例子：

```
3 2 4 1 2 3 4 1
I   I       I
```

它的前缀最大值在如上标号的三个地方出现,所以这个就可以被划分成三段：

```
[3 2] [4 1 2 3] [4 1]
```

那么怎么判断这个分段是否合法呢？首先，一个充要条件就是：一个段内没有相同元素。因为每一段是在一个排列里的，如果排列里出现两个相同的数就出问题了。

如果你认为只有这样，那就太简单了，有个反例可以秒掉：

```
[2 1] [3 1] [3 2]
```

显然这个是无法分割的。

所以我们把分段当中元素有相同的元素的段连一条边，那么这里要构成一张二分图才能保证有合法方案。上面的例子就是构成了一个三元环，所以无法划分。

于是这题就做完了，当然详细看代码：
```cpp
#include <bits/stdc++.h>

using namespace std;

bool basic_check(const vector<int>& t) {
  int n = ((int)t.size() - 1) / 2;
  vector<int> cnt(n + 1);
  for (int i = 1; i <= 2 * n; i++) cnt[t[i]] += 1;
  for (int i = 1; i <= n; i++) {
    if (cnt[i] != 2) {
      return false;
    }
  }
  return true;
}

int main() {
  ios::sync_with_stdio(false);
  cin.tie(0);
  int T;
  cin >> T;
  while (T--) {
    int n;
    cin >> n;
    vector<int> t(2 * n + 1);
    for (int i = 1; i <= 2 * n; i++) cin >> t[i];
    if (!basic_check(t)) {
      cout << "NO\n";
      continue;
    }
    int pre_max = 0;
    vector<int> max_pos;
    for (int i = 1; i <= 2 * n; i++) {
      pre_max = max(pre_max, t[i]);
      if (t[i] == pre_max) {
        max_pos.push_back(i);
      }
    }
    max_pos.push_back(2 * n + 1);
    vector<int> seg(2 * n + 1);
    for (int i = 0; i < (int)max_pos.size() - 1; i++) {
      for (int j = max_pos[i]; j < max_pos[i + 1]; j++) {
        seg[j] = i + 1;
      }
    }
    vector<vector<int>> pos(n + 1);
    for (int i = 1; i <= 2 * n; i++) {
      pos[t[i]].push_back(i);
    }
    bool good = true;
    for (int i = 1; i <= n; i++) {
      if (seg[pos[i][0]] == seg[pos[i][1]]) good = false;
    }
    if (good) {
      int k = (int)max_pos.size() - 1;
      vector<vector<int>> G(k + 1);
      for (int i = 1; i <= n; i++) {
        int a = seg[pos[i][0]];
        int b = seg[pos[i][1]];
        G[a].push_back(b);
        G[b].push_back(a);
      }
      for (int i = 1; i <= k; i++) {
        sort(G[i].begin(), G[i].end());
        G[i].erase(unique(G[i].begin(), G[i].end()), G[i].end());
      }
      vector<int> col(k + 1);
      auto bfs = [&](int x) -> bool {
        queue<int> Q;
        Q.push(x);
        col[x] = 1;
        while (!Q.empty()) {
          int cur = Q.front(); Q.pop();
          for (auto nx: G[cur]) {
            if (col[nx] == col[cur]) return false;
            if (col[nx] == 0) {
              col[nx] = -col[cur];
              Q.push(nx);
            }
          }
        }
        return true;
      };
      for (int i = 1; i <= k; i++) {
        if (!col[i] && !bfs(i)) good = false;
      }
    }
    cout << (good ? "YES\n" : "NO\n");
  }
  return 0;
}
```
## C.安放皇冠 

先咕咕着