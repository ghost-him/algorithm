# [蓝桥杯 试题 算法提高 搬运冰块（C ++）](http://lx.lanqiao.cn/problem.page?gpid=T2996)

## 题目浏览
**资源限制**
时间限制：1.0s   内存限制：256.0MB

**问题描述**
　　丑枫接到了一份奇葩的工作:往冰库里搬运冰块.冰库外放着N箱冰块,由于室外温度高,冰块会很快融化,且每箱冰块的融化速度不同.因为每箱冰块的体积,质量不等,把每箱冰块搬运进冰块花费的时间也不同.因此需要合理安排搬运顺序,才能使总的冰块融化量最小.丑枫请你帮忙计算最少的总融化量是多少,以便汇报上司.
　　
**输入格式**
　　第一行输入整数N
　　接下来N行,每行两个整数,分别表示每箱冰块的搬运耗时Ti及融化速度Di.
　　
**输出格式**
　　输出最少的总融化量
　　
**样例输入**
6
6 1
4 5
4 3
6 2
8 1
2 6

**样例输出**
86

**数据规模和约定**
　　2<=N<=100000,1<=Ti<=4000000,1<=Di<=100
　　
**样例说明**
　　按照6、2、3、4、1、5的顺序搬运

## 算法浏览

```cpp
#include<iostream>
#include<algorithm>

using namespace std;

const int N = 100010;

struct Node{
    int t, d;
    bool operator<(const struct Node & n) const{
        return t * n.d < n.t * d;
    }
}node[N];

int n;

int main()
{
    cin >> n;
    for(int i = 0; i < n; i++){
        int a,b;
        scanf("%d%d",&a,&b);
        node[i].t = a;
        node[i].d = b;
    }

    std::sort(node,node + n);

    long long time = 0;
    long long res = 0;
    for(int i = 0; i <= n; i++)
    {
        res += time * node[i].d;
        time += node[i].t;
    }
    cout<< res << endl;
}

```

## 核心思路
由于求的是最小的融化的体积，所以可以以融化体积为基准来排序

设 i 个冰块的搬运时间为 t1, 融化速度为 d1,第 i + 1个冰块的搬运时间为 t2,融化速度为d2，

则 若先搬运第i个冰块，则融化的体积为 t1 * d2，若先搬运第 i + 1个冰块，则融化的体积为t2 * d1 