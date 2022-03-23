# [蓝桥杯 试题 算法提高 最大区间和(C++)](http://lx.lanqiao.cn/problem.page?gpid=T2684)

## 题目浏览
**资源限制**
时间限制：1.0s   内存限制：256.0MB

**问题描述**
　　给定n个数A1,A2,A3……An，求两个数l,r满足l<=r并最大化Al+A(l+1)+……Ar，输出这个最大值
　　
**输入格式**
　　第一行一个数n，接下来一行有n个用空格隔开的数，第i个数表示Ai
　　
**输出格式**
　　输出仅一行，即最大区间和
　　
**样例输入**
4
-1 -2 -3 4

**样例输出**
4

**数据规模和约定**
　　每个数绝对值不超过2^30;
　　n<=1000000

## 算法浏览

```cpp
#include<iostream>
#include<algorithm>

using namespace std;

const int N = 1000010;

int n;
long long dp[N];
long long ans = -1e9;

int main()
{
    cin >> n;
    for(int i = 1; i <= n; i++)
    {
        long long temp;
        scanf("%lld", &temp);
        dp[i] = max(dp[i - 1] + temp, temp);
        ans = max(ans,dp[i]);
    }
    cout << ans <<endl;
    return 0;
}
```
## 核心思路
由于看到数据范围 n<=1000000 ,可知,这个算法一定会是一个O(n)的算法

这边可以使用动态规划来解决这个题,由于是个区间,所以可以转换为最大连续的字段和