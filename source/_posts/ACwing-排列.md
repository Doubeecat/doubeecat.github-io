---
title: ACwing 92 93 94 解题报告
tags:
  - 递归
categories:
  - 解题报告
mathjax: true
date: 2020-04-06 13:59:29
description:
---


> [递归实现指数型枚举](<https://www.acwing.com/problem/content/94/>)

> [递归实现组合型枚举](<https://www.acwing.com/problem/content/95/>)

> [递归实现排列型枚举](<https://www.acwing.com/problem/content/96/>)


> 指数型：在 $1-n$ 这 $n$ 个整数中随机选取任意多个，输出所有可能的次序。

> 组合型：从 $1- n$ 这 $n$ 个整数中随机选出 $m$ 个，输出所有可能的选择方案。

> 排列型：把 $1-n$ 这 $n$ 个整数排成一行后随机打乱顺序，输出所有可能的次序。

<!-- more -->

## 解题思路：

这三题的思路大致一致，所以放在一起来写。

指数型：很明显，这个问题可以转化成：

有 $n$ 个整数，每个整数可以选可以不选，求总共有几种方案。

我们可以使用递归来求解问题，每次递归时，我们有两条路：“选"，”不选”

放在代码里就是这样：

```cpp
v.push_back(p);//选择这个数
calc(x+1);//选择这个数的分支
v.pop_back();//还原现场

calc(x+1);//不选这个数的分支
```

而每次递归后，我们面对的问题规模（还剩几个数需要选）-1，变成一个规模更小的问题，所以可以用递归求解。

组合型：这个问题和上一个本质一样，我们只要加入更多的限制条件：

1. 当当前选取的数数量大于 $m$ 时，不再选数。
2. 当剩下的数全选也不够 $m$ 时，不再选数。

放在代码里就是这样：

```cpp
if (v.size() > m || v.size + (n-x+1) < m) return ;
```

排列性：这里，我们的问题就变成了：

把 $n$ 个整数按照任意次序排列

递归中，我们每次选取一个数放在当前位置，问题就变成了：把 $n-1$ 个整数按照任意次序排列，所以可以用递归来做

放在代码里就是这样：

```cpp
for (int i = 1;i <= n;++i) {
		if (!chos[i]) {
			ord[x] = i;
			chos[i] = 1;
			calc(x+1);
			chos[i] = 1;
		}
	}
```



## 代码：

 指数型：

```cpp
int x;
vector <int> chosen;

void choice(int n) {
    if (n == x+1) {
        fp(i,0,chosen.size()) {
            printf("%d ",chosen[i]);
        }
        printf("\n");
        return ;
    }
    choice(n+1);
    chosen.push_back(n);
    choice(n+1);
    chosen.pop_back();
}

int main() {
    x = read();
    choice(1);
    return 0;
}
```

组合型：

```cpp

std::vector<int> v;

int n,m;

void c(int x) {
	if (v.size() == m) {
		for (size_t i = 0;i < v.size();++i) {
			printf("%d ",v[i]);
		}
		puts("");
		return ;
	}
	if (v.size() > m || v.size() + (n - x + 1) < m) {//判断边界，第一个是判断当前是否大于m个了，第二个判断是否选不够
		return ;
	}	
	v.push_back(x);
	c(x+1);
	v.pop_back();
	c(x+1);

}

signed main() {
	n = read(),m = read();
	c(1);
	return 0;
}
```

排列型：

```cpp
int n,ord[10];
bool chos[10];

void calc(int x) {
	if (x == n+1) {
		for (int i = 1;i <= n;++i) {
			printf("%d ",ord[i]);
		}
		puts("");
		return ;
	}
	for (int i = 1;i <= n;++i) {
		if (!chos[i]) {
			ord[x] = i;
			chos[i] = 1;
			calc(x+1);
			chos[i] = 1;
		}
	}
}

signed main() {
	n = read();
	calc(1);
	return 0;
}
```

