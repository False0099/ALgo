# 知识点
  ## [[单调队列]] [[动态规划]]
# 题目
 输入一个长度为 $n$  的整数序列，从中找出一段长度不超过 $m$  的连续子序列，使得子序列中所有数的和最大。

**注意：** 子序列的长度至少是 1。

#### 输入格式

第一行输入两个整数 n,m。

第二行输入 n 个数，代表长度为 n 的整数序列。

同一行数之间用空格隔开。

#### 输出格式

输出一个整数，代表该序列的最大子序和。

#### 数据范围

$1≤n,m≤300000$  
保证所有输入和最终结果都在 int 范围内。

#### 输入样例：

```
6 4
1 -3 5 1 -2 3
```

#### 输出样例：

```
7
```

# 思路
·我们可以按照所求的子序列的最后一位进行分类，可以划分为最后一位在 0，1，2，.......n 然后我们在固定最后一位的情况下，统计各个长度下数组和的最大值（这一部可以用单调队列优化）

# AC 代码
```cpp
#include <iostream>
#include <cstring>
#include <algorithm>

using namespace std;

typedef long long LL;

const int N = 3e5 + 10;

int n, m;
LL s[N], que[N];

int main()
{
    scanf("%d%d", &n, &m);
    for (int i = 1; i <= n; i ++ ) scanf("%lld", &s[i]), s[i] += s[i - 1];//初始化前缀和数组

    LL res = -1e18;
    int hh = 0, tt = 0; que[0] = 0;//初始化单调队列
    for (int i = 1; i <= n; i ++ )
    {
        while (hh <= tt && i - que[hh] > m) hh ++ ;
        res = max(res, s[i] - s[que[hh]]);
        while (hh <= tt && s[que[tt]] >= s[i]) tt -- ;
        que[ ++ tt] = i;
    }
    printf("%lld\n", res);
    return 0;
}

作者：一只野生彩色铅笔
链接：https://www.acwing.com/solution/content/67527/
来源：AcWing
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
```
# 备注
