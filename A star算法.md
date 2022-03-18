# A* star算法

> 与**`dijkstra算法`**相似

**思想**

对宽搜的一个优化: 若所有的边权都是非负的, 那么就可以用启发函数来优化BFS的过程



> 启发函数只是改变了状态搜索的顺序, 优先搜索最有可能是答案的状态, 从而减少搜索的状态数

**与`dijkstra算法`的区别**

* `dijkstra`寻找的是从某个点到所有点的最小的距离
* `A* star`寻找的是从某一个点开始到目标点的最小值的距离



**特点**

* 图中的点数**特别多**, 多到无法将所有点的最短距离求出来



**核心的公式**

`dist[s] + f(s) // 当前点到起点的距离 + 当前点到终点的估计距离`

==只要 **f(s)** 小于当前点到终点的真实的距离, 就可以保证, 当某个状态s从优先队列中出来的时候, 它的距离一定是最短距离==



* 当`f(s) == 0`时, 则退化为`dijkstra算法`

* 当`f(s) == g(s)`时,则为线性算法 , `g(s)`为当前点到终点的真实的距离



**题目示例**

[第K短路](https://www.acwing.com/problem/content/180/)

给定一张 N 个点（编号  1,2…N ），M 条边的有向图，求从起点 S 到终点 T 的第 K 短路的长度，路径允许重复经过点或边。

**输入格式**
第一行包含两个整数 N 和 M。

接下来 M 行，每行包含三个整数 A,B 和 L，表示点 A 与点 B 之间存在有向边，且边长为 L。

最后一行包含三个整数 S,T 和 K，分别表示起点 S，终点 T 和第 K 短路。

**输出格式**
输出占一行，包含一个整数，表示第 K 短路的长度，如果第 K 短路不存在，则输出 −1。

**数据范围**

1 ≤ S,T ≤ N ≤ 1000 ,
0 ≤ M ≤ 10^4^,
1 ≤ K ≤ 1000,
1 ≤ L ≤ 100
**输入样例：**

```
2 2
1 2 5
2 1 4
1 2 2
```

**输出样例：**

```
14
```





**代码**

```cpp
#include<iostream>
#include<algorithm>
#include<queue>
#include<cstring>

using namespace std;

typedef pair<int, int> PII;
typedef pair<int, PII> PIII;

const int N = 1010, M = 200010;

int n, m;
int h[N], rh[N], e[M], ne[M], idx, w[M];					// 数组模拟链表
int dist[N], f[N], st[N];													// 到起点的真实的距离, 到终点的估计的距离,被遍历过的次数
int S, T, K;																			// 起点, 终点, 被遍历的次数

void add(int *h, int a, int b, int c) {
    e[idx] = b, w[idx] = c, ne[idx] = h[a], h[a] = idx++;
}

void dijkstra() {																	// 用dijkstra求每个点到终点的最短的距离, 并将其作为估计距离
    priority_queue<PII, vector<PII>, greater<PII>> heap;
    memset(dist, 0x3f,sizeof dist);
    dist[T] = 0;
    heap.push({0, T});

    while (heap.size()) {
        auto t = heap.top();
        heap.pop();

        int ver = t.second;
        if (st[ver]) continue;
        st[ver] = 1;
        for (int i = rh[ver]; ~i; i = ne[i]) {
            int j = e[i];
            if (dist[j] > dist[ver] + w[i]) {
                dist[j] = dist[ver] + w[i];
                heap.push({dist[j], j});
            }
        }
    }
    memcpy(f, dist, sizeof f);										// 作为估计距离
}

int a_star() {																		// a_star
    priority_queue<PIII, vector<PIII>, greater<PIII>> heap;	// 预测的距离, 真实的距离, 点
    heap.push({f[S], {0, S}}); // 估计值, 真实值, 点
    memset(st, 0, sizeof st);

    while (heap.size()) {
        auto t = heap.top();
        heap.pop();

        int ver = t.second.second;
        int distance = t. second.first;
        if (st[ver] >= K) continue;
        st[ver] ++;
        if (ver == T && st[ver] == K) {
            return distance;
        }

        for (int i = h[ver]; ~i; i = ne[i]) {
            int j = e[i];
            if (st[j] < K) {
                heap.push({distance + w[i] + f[j], {distance + w[i], j}});
            }
        }
    }
}

int main() {
    scanf("%d%d", &n, &m);
    memset(h, -1 ,sizeof h);														// 初始化
    memset(rh, -1, sizeof rh);
    while (m--) {
        int a, b, c;
        scanf("%d%d%d", &a, &b, &c);
        add(h, a, b, c);																// 添加点
        add(rh, b, a, c);
    }
    scanf("%d%d%d", &S, &T, &K);
    if (S == T) K++;																		// 如果起点和终点相同, 则需要多遍历一次

    dijkstra();																					// 求估计的距离

    printf("%d\n", a_star());
    return 0;
}
```

