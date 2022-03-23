# [蓝桥杯 试题 算法提高 网格贪吃蛇(C++)](http://lx.lanqiao.cn/problem.page?gpid=T2954)

## 题目浏览
**资源限制**
时间限制：1.0s   内存限制：256.0MB

**问题描述**
　　那个曾经风靡全球的贪吃蛇游戏又回来啦！这次贪吃蛇在m行n列的网格上沿格线爬行，从左下角坐标为(0,0)的格点出发，在每个格点处只能向上或者向右爬行，爬到右上角坐标为(m-1,n-1)的格点时结束游戏。网格上指定的格点处有贪吃蛇喜欢吃的豆豆，给定网格信息，请你计算贪吃蛇最多可以吃多少个豆豆。
　　
**输入格式**
　　输入数据的第一行为两个整数m、n（用空格隔开），分别代表网格的行数和列数；第二行为一个整数k，代表网格上豆豆的个数；第三行至第k+2行是k个豆豆的横纵坐标x、y（用空格隔开）。
　　
**输出格式**
　　程序输出一行，为贪吃蛇可吃豆豆的最大数量。
　　
**样例输入**
10 10
10
3 0
1 5
4 0
2 5
3 4
6 5
8 6
2 6
6 7
3 1

**样例输出**
5

**数据规模和约定**
　　1≤m, n≤10^6，0≤x≤m-1，0≤y≤n-1，1≤k≤1000

## 算法浏览

```cpp
#include<iostream>
#include<algorithm>
#include<map>
#include<vector>
#include<set>

#define x first
#define y second

using namespace std;

typedef pair<int, int> PII;

const int N = 1010;

int n, m, k;
int g[N][N];
int dp[N][N];
vector<PII> node;
map<int, int> dx;//x对应的坐标
map<int, int> dy;//y对应的坐标

int main()
{
    cin >> m >> n >> k;
    for(int i = 0; i < k; i++)//读入每个点
    {
        int x, y;
        scanf("%d%d", &x, &y);
        PII temp(x,y);
        node.push_back(temp);
        dx[x] = 0;
        dy[y] = 0;
    }

    int nx,ny;//映射以后的长度和宽度
    int j = 1;
    for(map<int, int> :: iterator i = dx.begin(); i != dx.end(); i++, j++) i->second = j;//设置每个值对应的映射值 x坐标
    nx = j - 1;

    j = 1;
    for(map<int, int> :: iterator i = dy.begin(); i != dy.end(); i++, j++) i->second = j;//设置每个值对应的映射值 y坐标
    ny = j - 1;
    
    for(int i = 0; i < node.size(); i++)//将映射后的坐标在图上标注出来
    {
        PII temp = node[i];
        g[dx[temp.x]][dy[temp.y]] = 1;
    }
	//动态规划
    for(int i = 1; i <= nx; i++){
        for(int j = 1; j <= ny; j++)
        {
            dp[i][j] = g[i][j];
            dp[i][j] += max(dp[i - 1][j], dp[i][j - 1]);
        }
    }

    cout << dp[nx][ny] << endl;
    return 0;
}
```


## 核心思路

首先先看一下这个数据范围，可以知道，地图很大，但是点数很稀疏，这个情况怎么办？
若将每个点直接在地图上标注出来以后会浪费大量的时间在计算没有意义的点（实际上也根本开不出来这么大的地图）

所以可以将根本没有用到的行和列剔除掉，换一种思路，即计算各个点之间的相对的位置
。
举个例子(2, 5), (1000, 633), (1000,800), (2560,48520)可以映射成(1,1), (2,2), (2,3), (3, 4)这样就可以将一个很大的地图变成一个很小的地图，再在这个很小的地图上做动态规划可以通过用例。