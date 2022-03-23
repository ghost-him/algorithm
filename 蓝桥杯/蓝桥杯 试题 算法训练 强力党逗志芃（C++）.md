# [蓝桥杯 试题 算法训练 强力党逗志芃（C++）](http://lx.lanqiao.cn/problem.page?gpid=T2963)

## 题目浏览
**资源限制**
时间限制：1.0s   内存限制：256.0MB

**问题描述**
　　逗志芃励志要成为强力党，所以他将身上所以的技能点都洗掉了重新学技能。现在我们可以了解到，每个技能都有一个前提技能，只有学完了前提技能才能学习当前的技能（有一个最根本的技能不需要前提技能）。学习每个技能要消耗一个技能点，然后可以获得这个技能的威力值。由于逗志芃要陪妹子，所以他希望你教他如何点技能使得威力值最大从而成为强力党。
　　
**输入格式**
　　第一行两个数n，m表示有n个技能和m个技能点。第二行有n个数，第i个数表示第i个技能的威力值。
　　之后的n-1行，每行两个数x,y，表示y技能的前提技能是x，也就是说先学第x个技能才能学弟y个技能。
　　
**输出格式**
　　一个数，最大的威力值。
　　
**样例输入**
3 2
1 10 20
1 2
1 3

**样例输出**
21

**数据规模和约定**
　　0<n,m<=200, 技能的威力值不超过200。

## 算法代码

```cpp
#include<iostream>
#include<algorithm>
#include<cstring>
using namespace std;

const int N = 210;

int n,m;
int q[N];
int h[N],e[N],ne[N],idx;
int dp[N][N];//以i为根节点，总花费的点数不超过j的最大方案
bool st[N];//是否有前驱
int res = 0;//伤害的最大的值


void add(int x,int y)
{
    e[idx] = y,ne[idx] = h[x],h[x] = idx++;
}

void dfs(int u)
{
    for(int i = h[u];i != -1; i = ne[i])//循环物品组
    {
        int son = e[i];
        dfs(son);

        for(int j = m - 1; j >= 0; j--)//循环剩余的点数
            for(int k = 0; k <= j; k++)//循环决策
                dp[u][j] = max(dp[u][j], dp[u][j - k] + dp[son][k]);
    }

    //将当前的节点1
    for(int i = m; i >= 1; i--) dp[u][i] = dp[u][i - 1] + q[u];
    dp[u][0] = 0;
}

int main()
{
    cin >> n >> m;
    memset(h, -1 ,sizeof h);

    for(int i = 1; i <= n; i++)//读取每个节点的值
        cin>>q[i];

    for(int i = 1; i < n; i++)//读取节点之间的关系
    {
        int x,y;
        cin >> x >> y;
        add(x,y);//x为y的父节点
        st[y] = true;
    }

    int root;//寻找根节点
    for(int i = 1; i <= n; i++)
        if(st[i] == false){
            root = i;
            break;
        }
    
    dfs(root);
	
    cout << dp[root][m] << endl;

    return 0;
}
```

## 算法核心思路
这个题目属于 树形动态规划、有依赖的背包问题 

技能与技能之间有从属关系，所以可以将节点变成一个树形结构

`dp[i][j]` 表示以i为根节点(选择的技能)，容积为j(能用的技能点数)的背包最多能装下的价值(伤害最高)

状态转移可以看成是分组背包问题（以子节点作为一组，每组里面有若干个体积的划分，且每个体积只能选一次，选择价值最大的那个体积），以体积作为划分的依据（如果是以点为划分的依据，那么时间复杂度会很高）