# [蓝桥杯 试题 算法提高 学霸的迷宫(C++)](http://lx.lanqiao.cn/problem.page?gpid=T291)

## 题目描述
**资源限制**
时间限制：1.0s   内存限制：256.0MB

**问题描述**
　　学霸抢走了大家的作业，班长为了帮同学们找回作业，决定去找学霸决斗。但学霸为了不要别人打扰，住在一个城堡里，城堡外面是一个二维的格子迷宫，要进城堡必须得先通过迷宫。因为班长还有妹子要陪，磨刀不误砍柴功，他为了节约时间，从线人那里搞到了迷宫的地图，准备提前计算最短的路线。可是他现在正向妹子解释这件事情，于是就委托你帮他找一条最短的路线。
　　
**输入格式**
　　第一行两个整数n， m，为迷宫的长宽。
　　接下来n行，每行m个数，数之间没有间隔，为0或1中的一个。0表示这个格子可以通过，1表示不可以。假设你现在已经在迷宫坐标(1,1)的地方，即左上角，迷宫的出口在(n,m)。每次移动时只能向上下左右4个方向移动到另外一个可以通过的格子里，每次移动算一步。数据保证(1,1)，(n,m)可以通过。
　　
**输出格式**
　　第一行一个数为需要的最少步数K。
　　第二行K个字符，每个字符∈{U,D,L,R},分别表示上下左右。如果有多条长度相同的最短路径，选择在此表示方法下字典序最小的一个。
　　
**样例输入**
Input Sample 1:
```
3 3
001
100
110
```
Input Sample 2:
```
3 3
000
000
000
```
**样例输出**
Output Sample 1:
```
4
RDRD
```
Output Sample 2:
```
4
DDRR
```
**数据规模和约定**
　　有20%的数据满足：$1<=n,m<=10$
　　有50%的数据满足：$1<=n,m<=50$
　　有100%的数据满足：$1<=n,m<=500$

## 算法
```c++
#include<iostream>
#include<algorithm>
#include<cstring>
#include<queue>
#include<string>

#define x first
#define y second

using namespace std;

#define PII pair<int, int>

const int N = 510;

int n, m;
char g[N][N];
int dist[N][N];
PII last[N][N];
char lasta[N][N];

string bfs() {
    queue<PII> store;
    store.push({1, 1});

    int dx[4] = {1,0,0,-1};
    int dy[4] = {0,-1,1,0};
    char D[4] = {'D','L','R','U'};
    while (store.size()) {
        PII temp = store.front();
        store.pop();

        if (temp.x == n && temp.y == m) {
            break;
        }

        for (int i = 0; i < 4; i++) {
            int nx = temp.x + dx[i];
            int ny = temp.y + dy[i];
            if (nx < 1 || nx > n || ny < 1 || ny > m) continue;
            if (dist[nx][ny] != 0 || g[nx][ny] == '1') continue;
            dist[nx][ny] = dist[temp.x][temp.y] + 1;
            last[nx][ny] = {temp.x, temp.y};
            lasta[nx][ny] = D[i];
            store.push({nx, ny});
        }
    }
    string res = "";
    int nx = n, ny = m;
    while (nx != 1 || ny != 1) {
        res.push_back(lasta[nx][ny]);
        PII u = last[nx][ny];
        nx = u.x, ny = u.y;
    }

    reverse(res.begin(), res.end());
    return res;
}

int main() {
    cin >> n >> m;
    for (int i = 1; i <= n; i++)
        scanf("%s", g[i] + 1);
    string res = bfs();
    cout << dist[n][m] << endl;
    cout << res << endl;
    return 0;
}
```

## 核心
经典的bfs问题, 字典序确定宽搜的顺序(U, L, R, U)