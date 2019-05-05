---
title: 19-04-28 Dijkstra 最短路径算法
date: 2019-04-28 18:00:12
tags:
- 图
categories:
- 算法
---

###### 单源最短路问题

在带权图 *G*=(*V*,*E*) 中，每条边都有一个权值 *wi*，即边的长度。路径的长度为路径上所有边权之和。单源最短路问题是指：求源点 *s* 到图中其余各顶点的最短路径。

如果用我们之前学习的 dfs 来解决单源最短路问题，效率上会很慢，能解决的问题的数据规模非常小。

而 bfs 能解决的最短路问题只限制在边权为 1 的图上。对于边权不同的图，利用 bfs 求解最短路是错误的。



> 解决单源最短路径问题常用 Dijkstra 算法，用于计算一个顶点到其他所有顶点的最短路径。Dijkstra 算法的主要特点是以起点为中心，逐层向外扩展（这一点类似于 bfs，但是不同的是，bfs 每次扩展一个层，但是 Dijkstra 每次只会扩展一个点），每次都会取一个最近点继续扩展，直到取完所有点为止



非负权图 , 贪心思想

<!--more-->

######  算法流程

我们定义带权图 *G* 所有顶点的集合为 *V*，接着我们再定义**已确定从源点出发的最短路径的顶点集合**为 *U*，初始集合 *U* 为空，记**从源点 *s* 出发到每个顶点 *v* 的距离**为 *dist_v*，初始 *dist_s*=0，其他*dist_v*为∞。接着执行以下操作：

1. 从 *V*−*U* 中找出一个距离源点最近的顶点 *v*，将 *v* 加入集合 *U*。
2. 并用 dist_v 和点 *v* 连出的边来更新和 *v* 相邻的、不在集合 *U* 中的顶点的 dist，这一步称为松弛操作。
3. 重复步骤 1 和 2，直到 V=U或找不出一个从 *s* 出发有路径到达的顶点，算法结束。

如果最后V !=U，说明有顶点无法从源点到达；否则每个 dist_i表示从 *s* 出发到顶点 *i* 的最短距离。

Dijkstra 算法的时间复杂度为O(*V*^2)，其中 *V* 表示顶点的数量。



>  算法演示
>
> 接下来，我们用一个例子来说明这个算法。
>
> ![img](https://res.jisuanke.com/img/upload/20170428/15072f3ce9f3e53579a6c2e02d87ef57d56cb3fe.png)
>
> 
>
> 初始每个顶点的 dist 设置为无穷大 inf，源点 *M* 的 dist_M 设置为 0。
>
> 1. 当前*U*=∅，*V*−*U* 中 dist 最小的顶点是 M。
>
> 2. 从顶点 M 出发，更新相邻点的 dist。
>
> ![](https://res.jisuanke.com/img/upload/20170428/02b208d277615bebf57d9a796e46bc96900181a3.png)
>
> 
>
> 更新完毕，此时 *U*={*M*}，
>
> 1. *V*−*U* 中 dist 最小的顶点是 *W*。
> 2. 从 *W* 出发，更新相邻点的 dist。
>
> ![img](https://res.jisuanke.com/img/upload/20170428/a310ffeebb4ebd561660aeed7cf81db5448b98cc.png)
>
> 
>
> 更新完毕，此时 *U*={*M*,*W*}，
>
> 1. *V*−*U* 中 dist 最小的顶点是 *E*。
>
> 2. 从 *E* 出发，更新相邻顶点的 dist。
>
> ![img](https://res.jisuanke.com/img/upload/20170428/18bf6b9ce78f61fa85ce4b6563b8d3508b2b0470.png)
>
> 
>
> 更新完毕，此时 *U*={*M*,*W*,*E*}，
>
> 1. V*−*U* 中 dist最小的顶点是 *X*。
> 2. 从 *X* 出发，更新相邻顶点的 dist。
>
> ![img](https://res.jisuanke.com/img/upload/20170428/39744d7c33d595558c4c79205c7778b3ae476a01.png)
>
> 
>
> 更新完毕，此时 *U*={*M*,*W*,*E*,*X*}，
>
> 1. V*−*U* 中 dist 最小的顶点是 *D*。
> 2. 从 *D* 出发，没有其他不在集合 *U* 中的顶点。
>
> [![img](https://res.jisuanke.com/img/upload/20170428/ced8460a27686319529c898a1c7daec893bba313.png)](https://res.jisuanke.com/img/upload/20170428/ced8460a27686319529c898a1c7daec893bba313.png)
>
> 此时*U*=*V*，算法结束，单源最短路计算完毕。



```c++
#include <iostream>
#include <cstring>

using namespace std;

const int N = 1001;
const int M = 10001;
const int INF = 0x3f3f3f3f;

struct edge {
    int v, w, next;
    edge() {}
    edge(int _v, int _w, int _next): v(_v), w(_w), next(_next) {}
} e[M * 2];

int head[N];
int cnt = 0; //链表大小

void insert(int u, int v, int w) {
    e[cnt] = edge(v, w, head[u]);
    head[u] = cnt++;
}

void insert2(int u, int v, int w) {
    insert(u, v, w);
    insert(v, u, w);
}

void init() {
    memset(head, -1, sizeof head);
    cnt = 0;
}

int n, m;
int dis[N];
bool vis[N];

void dijkstra(int u) {
    memset(vis, false, sizeof vis);
    memset(dis, 0x3f, sizeof dis);
    dis[u] = 0;
    for (int i = 0; i < n; i++) {
        int min_dis = INF, min_index = -1;
        for (int j = 1; j <= n; j++) { //j 从0 还是从 1 根据情况而定
            if (!vis[j] && dis[j] < min_dis) {
                min_index = j;
                min_dis = dis[j];
            }
        }
        if (min_dis == -1) { //图不是连通图
            return;
        }
        vis[min_index] = true;
        for (int j = head[min_index]; ~j; j = e[j].next) { //去便利当前dis最小点的所有邻接点（便利链表）
            int v = e[j].v;
            int w = e[j].w;
            if (!vis[v] && dis[v] > dis[min_index] + w) {
                dis[v] = dis[min_index] + w;
            }
        }
    }
}

int main() {
    init();
    int u, v, w;
    cin >> n >> m;
    while (m--) {
        cin >> u >> v >> w;
        insert2(u, v, w);
    }
    dijkstra(1);
    cout << dis[n] << endl;
    return 0;
}
```

