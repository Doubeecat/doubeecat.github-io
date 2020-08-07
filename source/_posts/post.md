---
title: 图论入门
mathjax: true
date: 2020-08-05 20:47:00
tags: 图论,MST,最短路,Tarjan
categories: 学习笔记
description: 
---

[经 典 老 番](https://www.zzlblog.ga/2019/08/11/%E5%9B%BE%E8%AE%BA%E6%9D%BF%E5%AD%90/)

<!--more-->

## 最小生成树(MST)

### Kruskal

一种基于贪心和并查集的最小生成树算法。

最开始是 $n$ 个点的森林,每次选择最小的一条边,判断边端点是否联通,如果不连通就加到一起。

时间复杂度 $O(m \log m)$ ,主要复杂度在排序。

#### 代码

```cpp
int fa[N];
int find(int x) {return x == fa[x] ? x : fa[x] = find(fa[x]);}

struct edge {
    int u,v,w;
    friend inline bool operator < (const edge & a,const edge & b) {
        return a.w < b.w;
    }
}Edge[M];

int Kruskal() {
    std::sort(Edge+1,Edge+1+m);
    int ans = 0;
    for (int i = 1;i <= m;++i) {
        edge e = Edge[i];
        int fu = find(e.u),fv = find(e.v);
        if (fu != fv) {
            ans += e.w;
            fa[fu] = fv;
        }
    }
    return ans;
}
```

### Prim 

从一个点开始建树,每次把连接树外和树内最小的边加入树。

如果在稀疏图中可以采用堆优化,代码类似 Dijkstra。

时间复杂度 $O(n^2 + m)$ 常用于稠密图,优化后为 $O((n + m) \log n)$

```cpp
int to[M],hd[N],nxt[M],edg[M],tot;
inline void add(int u,int v,int w) {to[++tot] = v;edg[tot] = w;nxt[tot] = hd[u];hd[u] = tot;}
inline void addedge(int u,int v,int w) {add(u,v,w),add(v,u,w);}

int dis[N],n,m;
bool vis[N];

int Prim() {
    memset(dis,0x3f,sizeof dis);
    memset(vis,0,sizeof vis);
    dis[1] = 0;
    for (int i = hd[1];i;i = nxt[i]) {
        dis[to[i]] = min(dis[to[i]],edg[i]);
    }
    
    int now = 1,ans = 0;
    for (int tot = 1;tot < n;++tot) {
        int minn = INF;
        vis[now] = 1;
        //最开始森林里没有东西 找一个最小的点插入
        for (int i = 1;i <= n;++i) {
            if (!vis[i] && minn > dis[i]) {
                minn = dis[i];
                now = i;
                //printf("minn:%d now:%d \n",minn,now);
            }
        }
        ans += minn;
       // printf("ans:%d\n",ans);
        for (int i = hd[now];i;i = nxt[i]) {
            if (dis[to[i]] > edg[i] && !vis[to[i]]) {
                dis[to[i]] = edg[i];
            }
        }
    }
    return ans;
}
```