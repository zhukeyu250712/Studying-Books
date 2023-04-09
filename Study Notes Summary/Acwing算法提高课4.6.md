# 算法（2022-02-13）

[TOC]

## ♥♥

## 一.动态规划

### 1.数字三角形模型

#### 1.1  摘花生

**[题目：AcWing1015. 摘花生]()**

**题目描述**

有 N 件物品和一个容量是 V 的背包。每件物品只能使用一次。

第 i 件物品的体积是 vi，价值是 wi。

求解将哪些物品装入背包，可使这些物品的总体积不超过背包容量，且总价值最大。
输出最大价值。

**输入格式**

第一行两个整数，N，V，用空格隔开，分别表示物品数量和背包容积。

接下来有 N 行，每行两个整数 vi,wi，用空格隔开，分别表示第 i 件物品的体积和价值。

**输出格式**

输出一个整数，表示最大价值。

**数据范围**

$0 < N,V ≤ 1000$

$0<vi,wi≤1000$

**输入样例：**

```c
4 5
1 2
2 4
3 4
4 5
```

**输出样例：**

```c
8
```

**题解：**

``` c
DP  1.状态(一个集合)表示
		二维：f(i,j)
		1.1 集合：所有选法
				条件：（1）所有从(1,1)走到(i,j)的所有路线
					 （2）所有路线的最大值max
    
		1.2 属性：max值
    
	2.状态计算:一步一步的将状态计算出来
		(1)总结：
        	1)集合的划分原则：
        		a.重复(不一定需要必须满足)
        		b.不遗漏(必须满足)
        	2)很重要的划分依据"最后"
        
		(2)f(i,j)集合划分为若干个更小的子集，每个子集都能求出答案
            1)最后一步是从上面下来的
        		(1,1)-->(i-1,j)-->(i,j)
        		f[i-1][j]+w[i][j]
            2)最后一步是从左边下来的
        		(1,1)-->(i,j-1)-->(i,j)
        		f[i][j-1]+w[i][j]
		  
		最终f[i][j]=max(f[i-1][j]+w[i][j],f[i][j-1]+w[i][j])
    3.优化
    4.注意       
```

**代码：**

```c
#include <iostream>
#include <cstring>
#include <algorithm>
using namespace std;

const int N = 1010;
int n, m;
int v[N], w[N];
int f[N][N];

int main()
{
    cin >> n >> m;
    for (int i = 1; i <= n; i++)
        cin >> v[i] >> w[i];
    // f[0~n][0~m]
    // f[0][0~m]=0，一件物品也没有选，都为0,所以不用枚举
    for (int i = 1; i <= n; i++)
    {
        // j=0表示一共选了体积为0的物体
        for (int j = 0; j <= m; j++)
        {
            //选法不包含i
            f[i][j] = f[i - 1][j];
            //选法包含i,第i件物品体积小于等于背包总容量
            if (j >= v[i])
                f[i][j] = max(f[i][j], f[i - 1][j - v[i]] + w[i]);
        }
    }
    cout << f[n][m] << endl;
    return 0;
}
```

**优化后代码：**

```c
#include <iostream>
#include <cstring>
#include <algorithm>
using namespace std;

const int N = 1010;
int n, m;
int v[N], w[N];
int f[N];

int main()
{
    cin >> n >> m;
    for (int i = 1; i <= n; i++)
        cin >> v[i] >> w[i];
    // f[0~n][0~m]
    // f[0][0~m]=0，一件物品也没有选，都为0,所以不用枚举
    for (int i = 1; i <= n; i++)
    {
        // j=0表示一共选了体积为0的物体	
        for (int j = m; j >= v[i]; j--)
        {
            f[j] = max(f[j], f[j - v[i]] + w[i]);
        }
    }
    cout << f[m] << endl;
    return 0;
}
```



#### 1.2 完全背包问题

**[题目：AcWing 3. 完全背包问题]()**

**题目描述**

有 N 种物品和一个容量是 V 的背包，每种物品都有无限件可用。

第 i 种物品的体积是 vi，价值是 wi。

求解将哪些物品装入背包，可使这些物品的总体积不超过背包容量，且总价值最大。
输出最大价值。

**输入格式**

第一行两个整数，N，V，用空格隔开，分别表示物品种数和背包容积。

接下来有 N 行，每行两个整数 vi,wi，用空格隔开，分别表示第 i 种物品的体积和价值。

**输出格式**

输出一个整数，表示最大价值。

**数据范围**

$0 < N,V ≤ 1000$

$0 < vi,wi ≤ 1000$

**输入样例：**

```c
4 5
1 2
2 4
3 4
4 5
```

**输出样例：**

```c
10
```

**题解：**

> **结论**：
>
> 将0-1背包中j的循环顺序改成从小到大，就变成了完全背包。
>
> 0-1背包：`dp[i][j] = max(dp[i-1][j], dp[i-1][j-vi]+wi)`
>
> 完全背包：`dp[i][j] = max(dp[i-1][j], dp[i][j-vi]+wi)`

``` c
每件物品有无限个
DP 1.状态表示f(i,j)
    	1.1 集合
    		所有只考虑前i个物品，且总体积不大于j的所有选法
    	1.2 属性
    		Max
   2.状态计算
    	集合的划分:分若干组，按照第i个物品选多少个分
            0 1 2 3 4 5 ....k-1 k  第i个物品选了0~k个
            f(i,j)表示选第i个物品，总体积不超过j
    	(1)第i个物品不选，只考虑前1~i-1的话用f(i-1,j)表示
            当k=0,f(i-1,j)=f(i-1,j-k*v[i])+k*w[i]
        (2)第i个物品选，选k个
            1)去掉k个物品i
            2)求MAX,f(i-1,j-k*v[i])
            3)再加回来k个物品i
            =>f(i-1,j-k*v[i])+k*w[i]
     	=>f(i,j)=f(i-1,j-v[i]*k)+w[i]*k
            
   3.优化
   		f(i,j)=max(f(i-1,j) , f(i-1,j-v)+w , f(i-1,j-2*v)+2w , f(i-1,j-3*v)+3w ,.. )
        f(i,j-v)=max(         f(i-1,j-v)   , f(i-1,j-2*v)+2w , f(i-1,j-3*v)+3w ,.. )       =>f(i,j)=max(f(i-1,j),f(i-1,j-v)+w)
        
```

**代码：**

```c
#include <iostream>
#include <algorithm>
#include <cstring>

using namespace std;
const int N = 1010;

int n, m;
int v[N], w[N];
int f[N][N];

int main()
{
    cin >> n >> m;
    for (int i = 1; i <= n; i++)
        cin >> v[i] >> w[i];

    for (int i = 1; i <= n; i++)
        for (int j = 0; j <= m; j++)
            for (int k = 0; k * v[i] <= j; k++)
                f[i][j] = max(f[i][j], f[i - 1][j - v[i] * k] + w[i] * k);
    cout << f[n][m] << endl;
    return 0;
}
```

**优化代码**

```c
#include <iostream>
#include <algorithm>
#include <cstring>

using namespace std;
const int N = 1010;

int n, m;
int v[N], w[N];
int f[N][N];

int main()
{
    cin >> n >> m;
    for (int i = 1; i <= n; i++)
        cin >> v[i] >> w[i];

    for (int i = 1; i <= n; i++)
        for (int j = 0; j <= m; j++)
        {
            f[i][j] = f[i - 1][j];
            if (j >= v[i])
                f[i][j] = max(f[i][j], f[i][j - v[i]] + w[i]);
        }

    cout << f[n][m] << endl;
    return 0;
}
```

```c
#include <iostream>
#include <algorithm>
#include <cstring>

using namespace std;
const int N = 1010;

int n, m;
int v[N], w[N];
int f[N];

int main()
{
    cin >> n >> m;
    for (int i = 1; i <= n; i++)
        cin >> v[i] >> w[i];

    for (int i = 1; i <= n; i++)
        for (int j = v[i]; j <= m; j++)
            f[j] = max(f[j], f[j - v[i]] + w[i]);

    cout << f[m] << endl;
    return 0;
}
```



#### 1.3 多重背包问题

**[题目：AcWing 4. 多重背包问题 I]()**

**题目描述**

有 N 种物品和一个容量是 V 的背包。

第 i 种物品最多有 si 件，每件体积是 vi，价值是 wi。

求解将哪些物品装入背包，可使物品体积总和不超过背包容量，且价值总和最大。
输出最大价值。

**输入格式**

第一行两个整数，N，V，用空格隔开，分别表示物品种数和背包容积。

接下来有 N 行，每行三个整数 vi,wi,si，用空格隔开，分别表示第 i 种物品的体积、价值和数量。

**输出格式**

输出一个整数，表示最大价值。

**数据范围**

$0 < N,V ≤ 100$

$0 < vi,wi,si ≤ 100$

**输入样例：**

```c
4 5
1 2 3
2 4 1
3 4 3
4 5 2
```

**输出样例：**

```c
10
```

**题解：**

``` c
DP 1.状态表示f(i,j)
    1.1 集合
    	所有从前i个物品中选，并且总体积不超过j的选法
    
    1.2 属性
    	Max，总价值的最大值
    
    2.状态计算
    	集合的划分:分若干组，按照第i个物品选多少个分(完全背包类似)
            0 1 2 3 4 5 .... s[i]  第i个物品选了0~k个
            
		状态转移方程：f(i,j)=max(f[i][j],f[i-1][j-v[i]*k]+w[i]*k)  k=0,1,2.....s[i]
```

**代码：**

```c
#include <iostream>
#include <algorithm>
#include <cstring>

using namespace std;

const int N = 110;

int n, m;
int v[N], w[N], s[N];
int f[N][N];

int main()
{
    cin >> n >> m;
    for (int i = 1; i <= n; i++)
        cin >> v[i] >> w[i] >> s[i];

    for (int i = 1; i <= n; i++)
        for (int j = 0; j <= m; j++)
            for (int k = 0; k <= s[i] && k * v[i] <= j; k++)
                f[i][j] = max(f[i][j], f[i - 1][j - v[i] * k] + w[i] * k);

    cout << f[n][m] << endl;
    return 0;
}
```



**[题目：AcWing 5. 多重背包问题 II]()**

**题目描述**

有 N 种物品和一个容量是 V 的背包。

第 i 种物品最多有 si 件，每件体积是 vi，价值是 wi。

求解将哪些物品装入背包，可使物品体积总和不超过背包容量，且价值总和最大。

输出最大价值。

**输入格式**

第一行两个整数，N，V，用空格隔开，分别表示物品种数和背包容积。

接下来有 N 行，每行三个整数 vi,wi,si，用空格隔开，分别表示第 i 种物品的体积、价值和数量。

**输出格式**

输出一个整数，表示最大价值。

**数据范围**

$0 < N ≤ 1000$

$0 < V ≤ 2000$

$0 < vi,wi,si ≤ 2000$

**输入样例：**

```c
4 5
1 2 3
2 4 1
3 4 3
4 5 2
```

**输出样例：**

```c
10
```

**题解：**

``` c
DP 1.状态表示f(i,j)
    1.1 集合
    	所有从前i个物品中选，并且总体积不超过j的选法
    
    1.2 属性
    	Max，总价值的最大值
    
    2.状态计算
    	集合的划分:分若干组，按照第i个物品选多少个分(完全背包类似)
            0 1 2 3 4 5 .... s[i]  第i个物品选了0~k个
            
		状态转移方程：f(i,j)=max(f[i][j],f[i-1][j-v[i]*k]+w[i]*k)  k=0,1,2.....s[i]
    3.优化
    f(i,j)=max(f(i,j),f(i-1,j-v)+w,f(i-1,j-2v)+2w,......f(i-1,j-sv)+sw)
    f(i,j-v)=max(     f(i-1,j-v),  f(i-1,j-2v)+w, ......f(i-1,j-sv)+(s-1)w,
                 f(i-1,j-(s+1)v)+sw);
	
```

**代码**

```c
#include <iostream>
#include <algorithm>
#include <cstring>

using namespace std;

const int N = 110;

int n, m;
int v[N], w[N], s[N];
int f[N][N];

int main()
{
    cin >> n >> m;
    for (int i = 1; i <= n; i++)
        cin >> v[i] >> w[i] >> s[i];

    for (int i = 1; i <= n; i++)
        for (int j = 0; j <= m; j++)
            for (int k = 0; k <= s[i] && k * v[i] <= j; k++)
                f[i][j] = max(f[i][j], f[i - 1][j - v[i] * k] + w[i] * k);

    cout << f[n][m] << endl;
    return 0;
}
```

**代码优化：**

```c
#include <iostream>
#include <algorithm>
#include <cstring>

using namespace std;

const int N = 25000, M = 2010;

int n, m;
int v[N], w[M];
int f[N];

int main()
{
    cin >> n >> m;

    int cnt = 0;

    for (int i = 1; i <= n; i++)
    {
        int a, b, s;
        cin >> a >> b >> s;
        int k = 1;
        while (k <= s)
        {
            cnt++;
            v[cnt] = a * k;
            w[cnt] = b * k;
            s -= k;
            k *= 2;
        }
        if (s > 0)
        {
            cnt++;
            v[cnt] = a * s;
            w[cnt] = b * s;
        }
    }
    n = cnt;
    for (int i = 1; i <= n; i++)
        for (int j = m; j >= v[i]; j--)
            f[j] = max(f[j], f[j - v[i]] + w[i]);
    cout << f[m] << endl;
    return 0;
}
```

#### 1.4 分组背包问题

**[题目：AcWing 9. 分组背包问题]()**

**题目描述**

有 N 组物品和一个容量是 V 的背包。

每组物品有若干个，同一组内的物品最多只能选一个。

每件物品的体积是 vij，价值是 wij，其中 i 是组号，j 是组内编号。

求解将哪些物品装入背包，可使物品总体积不超过背包容量，且总价值最大。

输出最大价值。

**输入格式**

第一行有两个整数 N，V，用空格隔开，分别表示物品组数和背包容量。

接下来有 N 组数据：

- 每组数据第一行有一个整数 Si，表示第 i 个物品组的物品数量；
- 每组数据接下来有 Si 行，每行有两个整数 vij,wij，用空格隔开，分别表示第 i 个物品组的第 j 个物品的体积和价值；

**输出格式**

输出一个整数，表示最大价值。

**数据范围**

$0 < N,V ≤ 100$

$0 < Si ≤ 100$

$0 < vij,wij ≤ 100$

**输入样例：**

```c
3 5
2
1 2
2 4
1
3 4
1
4 5
```

**输出样例：**

```c
8
```

**题解：**

``` c
DP 1.状态表示f(i,j)
    1.1 集合
    	所有从前i组物品中选，并且总体积不大于j的选法
    
    1.2 属性
    	Max，总价值的最大值
    
   2.状态计算
    	集合的划分:分若干组，按照第i个物品选多少个分(完全背包类似)
            0 1 2 3 4 5 .... s[i]  选第i组的第k个物品选了0~k个
        (1)第i组的一个都不选
            f(i-1,j)
        (2)第i组物品选第k个物品
            f(i-1,j-v[i,k])+w[i,k]
           
```

**代码：**

```c
#include <iostream>
#include <algorithm>
#include <cstring>

using namespace std;

const int N = 110;
int n, m;
// s存每组的个数
int v[N][N], w[N][N], s[N];
int f[N];

int main()
{
    cin >> n >> m;

    for (int i = 1; i <= n; i++)
    {
        cin >> s[i];
        for (int j = 0; j < s[i]; j++)
            cin >> v[i][j] >> w[i][j];
    }

    for (int i = 1; i <= n; i++)
    {
        for (int j = m; j >= 0; j--)
        {
            for (int k = 0; k < s[i]; k++)
            {
                if (v[i][k] <= j)
                    f[j] = max(f[j], f[j - v[i][k]] + w[i][k]);
            }
        }
    }
    cout << f[m] << endl;
    return 0;
}
```



### 2.最长上升子序列模型

#### (1) 数字三角形

**[题目：AcWing 898. 数字三角形]()**

**题目描述**

给定一个如下图所示的数字三角形，从顶部出发，在每一结点可以选择移动至其左下方的结点或移动至其右下方的结点，一直走到底层，要求找出一条路径，使路径上的数字的和最大。

```r
        7
      3   8
    8   1   0
  2   7   4   4
4   5   2   6   5
```

**输入格式**

第一行包含整数n，表示数字三角形的层数。

接下来n行，每行包含若干整数，其中第 i 行表示数字三角形第 i 层包含的整数。

**输出格式**

输出一个整数，表示最大的路径数字和。

**数据范围**

$1≤n≤500,$
$−10000$≤三角形中的整数≤$10000$

**输入样例：**

```c
5
7
3 8
8 1 0 
2 7 4 4
4 5 2 6 5
```

**输出样例：**

```c
30
```

**题解：**

``` c
DP
    1.状态表示
    	1.1 集合：所有从起点，走到(i,j)的路径
    	1.2 属性: 所有路径上的数字之和的最大值Max
            
    2.状态计算：f(i,j)
        (1)来自左上方
            f(i-1,j-1)+a[i][j]
        (2)来自上面
            f(i-1,j)+a[i][j]
        (3)状态转移方程：f(i,j)=max(f(i-1,j-1)+a[i][j],f(i-1,j)+a[i][j])
        (4)时间复杂度=状态数量X转移的计算量  
```

**代码：**

```c
#include <iostream>
#include <algorithm>
#include <cstring>

using namespace std;

const int N = 510, INF = 1e9;
int n;
int a[N][N];
int f[N][N];

int main()
{
    cin >> n;
    for (int i = 1; i <= n; i++)
        for (int j = 1; j <= i; j++)
            cin >> a[i][j];

    //为了不处理边界，将f状态数组置为负无穷,从0开始,每行初始化为i+1个
    for (int i = 0; i <= n; i++)
        for (int j = 0; j <= i + 1; j++)
            f[i][j] = -INF;

    f[1][1] = a[1][1];

    for (int i = 2; i <= n; i++)
        for (int j = 1; j <= i; j++)
            f[i][j] = max(f[i - 1][j - 1] + a[i][j], f[i - 1][j] + a[i][j]);

    int res = -INF;

    for (int i = 1; i <= n; i++)
        res = max(res, f[n][i]);

    cout << res << endl;

    return 0;
}
```

#### (2) 最长上升子序列

**[题目：AcWing 895. 最长上升子序列]()**

**题目描述**

给定一个长度为N的数列，求数值严格单调递增的子序列的长度最长是多少。

**输入格式**

第一行包含整数N。

第二行包含N个整数，表示完整序列。

**输出格式**

输出一个整数，表示最大长度。

**数据范围**

$1≤N≤1000，$

$−10^9≤数列中的数≤10^9$

**输入样例：**

```c
7
3 1 2 1 8 5 6
```

**输出样例：**

```c
4
```

**题解：**

``` c
DP
    1.状态表示 f[i]
    	1.1 集合:所有以第i个数结尾的上升子序列的集合
    	1.2 属性：集合里面每一个上升子序列的长度的最大值Max
    2.状态计算
        集合划分f[i]:0 1 2 3 ....i-1
        (1)集合里面没有数
        (2)集合里面以第1个数结尾，以第2个数结尾，以第3个数结尾，...
        状态转移方程:a[i]=max(f[i],f[j]+1)  a[j]<a[i]  j=0,1,2,..i-1
        时间复杂度=状态数量X转移的计算量=n*n
```

**代码：**

```c
#include <iostream>
#include <algorithm>
#include <cstring>

using namespace std;

const int N = 1010;

int n;
int a[N], f[N];

int main()
{
    cin >> n;
    for (int i = 1; i <= n; i++)
        cin >> a[i];
    for (int i = 1; i <= n; i++)
    {
        f[i] = 1; //以i结尾的只有一个数a[i]
        for (int j = 1; j < i; j++)
        {
            if (a[j] < a[i])
                f[i] = max(f[i], f[j] + 1);
        }
    }

    int res = 0;
    for (int i = 1; i <= n; i++)
        res = max(res, f[i]);
    cout << res << endl;
    return 0;
}
```

**保存序列（存下状态转移）**

```c
//保存下来序列
#include <iostream>
#include <algorithm>
#include <cstring>

using namespace std;

const int N = 1010;

int n;
int a[N], f[N], g[N];

int main()
{
    cin >> n;
    for (int i = 1; i <= n; i++)
        cin >> a[i];
    for (int i = 1; i <= n; i++)
    {
        f[i] = 1; //以i结尾的只有一个数a[i]
        g[i] = 0; //表示只有一个数
        for (int j = 1; j < i; j++)
        {
            if (a[j] < a[i])
            {
                if (f[i] < f[j] + 1)
                {
                    f[i] = f[j] + 1;
                    //记录每个状态是从哪里转移来的
                    g[i] = j;
                }
            }
        }
    }

    //最优解下标
    int k = 1;
    for (int i = 1; i <= n; i++)
        if (f[k] < f[i])
            k = i;

    cout << f[k] << endl;
    for (int i = 0, le = f[k]; i < le; i++)
    {
        cout << a[k] << " ";
        k = g[k];
    }
    return 0;
}
```

#### (3)最长公共子序列

**[题目：AcWing 897. 最长公共子序列]()**

**题目描述**

给定两个长度分别为N和M的字符串A和B，求既是A的子序列又是B的子序列的字符串长度最长是多少。

**输入格式**

第一行包含两个整数N和M。

第二行包含一个长度为N的字符串，表示字符串A。

第三行包含一个长度为M的字符串，表示字符串B。

字符串均由小写字母构成。

**输出格式**

输出一个整数，表示最大长度。

**数据范围**

$1≤N,M≤1000$

**输入样例：**

```c
4 5
acbd
abedc
```

**输出样例：**

```c
3
```

**题解：**

``` c
DP
    1.状态表示f(i,j)
    	1.1 集合
    		所有在第一个序列的前i个字母中出现，且在第二个序列的前j个字母中出现的子序列
    	1.2 属性
    		所有长度的最大值Max
    		(1)求最大最小值答案可以重复，max(a,b)和max(b,c)得到max(a,b,c)
    		(2)求数量答案一定不能重复
    
    2.状态计算
    	集合划分：a[i],b[j]是否包含在子序列中
    			选a[i]选b[j],不选a[i]选b[j],选a[i]不选b[j],不选a[i]不选b[j]
    	(1)00 不选a[i]不选b[j]   f(i-1,j-1)这个序列一般不写，已经包含在了01和10的情况里面
    	(2)01 不选a[i]选b[j]	 f(i-1,j)不一定包含b[j],一定包含01的情况
    	(3)10 选a[i]不选b[j]	 f(i,j-1)
    	(4)11 选a[i]选b[j] 	  f(i-1,j-1)+1当a[i]==b[j]的时候
    	===>f(i,j)=max(01,10,11)
    	
```

**代码：**

```c
#include <iostream>
#include <cstring>
#include <algorithm>

using namespace std;
const int N = 1010;

int n, m;
char a[N], b[N];
int f[N][N];

int main()
{
    scanf("%d%d", &n, &m);
    scanf("%s%s", a + 1, b + 1);
    for (int i = 1; i <= n; i++)
    {
        for (int j = 1; j <= m; j++)
        {
            f[i][j] = max(f[i - 1][j], f[i][j - 1]);
            if (a[i] == b[j])
                f[i][j] = max(f[i][j], f[i - 1][j - 1] + 1);
        }
    }
    printf("%d\n", f[n][m]);
    return 0;
}
```



### 3.背包模型

**[题目：AcWing 282. 石子合并]()**

**题目描述**

设有N堆石子排成一排，其编号为1，2，3，…，N。

每堆石子有一定的质量，可以用一个整数来描述，现在要将这N堆石子合并成为一堆。

每次只能合并相邻的两堆，合并的代价为这两堆石子的质量之和，合并后与这两堆石子相邻的石子将和新堆相邻，合并时由于选择的顺序不同，合并的总代价也不相同。

例如有4堆石子分别为 1 3 5 2， 我们可以先合并1、2堆，代价为4，得到4 5 2， 又合并 1，2堆，代价为9，得到9 2 ，再合并得到11，总代价为4+9+11=24；

如果第二步是先合并2，3堆，则代价为7，得到4 7，最后一次合并代价为11，总代价为4+7+11=22。

问题是：找出一种合理的方法，使总的代价最小，输出最小代价。

**输入格式**

第一行一个数N表示石子的堆数N。

第二行N个数，表示每堆石子的质量(均不超过1000)。

**输出格式**

输出一个整数，表示最小代价。

**数据范围**

$1≤N≤300$

**输入样例：**

```c
4
1 3 5 2
```

**输出样例：**

```c
22
```

**题解：**

``` c
DP
    1.状态表示f(i,j)
    	1.1 集合：所有将第i堆石子到第j堆石子合并成一堆石子的合并方式
    	1.2 属性：所有合并方式的代价Min值
    2.状态计算
    	集合划分：以最后一次分界线的位置来划分，i到j有k=j-i+1个数
    	左边范围[i,k]，右边范围[k+1,j],将所有里面的最后一个数去了,加所有石子最后一次的总和代价
    	(1)左边1个，右边k-1个
    	(2)左边2个，右边k-2个
    		........
    	(k)左边k-1个，右边1个
    	==>f(i,j)=Min(f(i,k)+f(k+1,j)+s[j]-s[i-1])  k=i,i+1,...j-1
```

**代码：**

```c
#include <iostream>
#include <algorithm>
#include <cstring>

using namespace std;

const int N = 310;

int n;
int s[N];
int f[N][N];

int main()
{
    cin >> n;
    for (int i = 1; i <= n; i++)
        cin >> s[i];
    //求前缀和
    for (int i = 1; i <= n; i++)
        s[i] += s[i - 1];

    //先枚举长度,当区间长度是1的话代价为0，所以从0开始
    for (int len = 2; len <= n; len++)
    {
        //枚举起点
        for (int i = 1; i + len - 1 <= n; i++)
        {
            int l = i, r = i + len - 1;
            f[l][r] = 1e8;
            for (int k = l; k < r; k++)
            {
                f[l][r] = min(f[l][r], f[l][k] + f[k + 1][r] + s[r] - s[l - 1]);
            }
        }
    }
    cout << f[1][n] << endl;
    return 0;
}
```



### 4.状态机模型

**[题目：AcWing 900. 整数划分]()**

**题目描述**

一个正整数 n 可以表示成若干个正整数之和，形如：$n=n_1+n_2+…+nk_n$，其中 $n_1≥n_2≥…≥n_k,k≥1$。

我们将这样的一种表示称为正整数 n 的一种划分。

现在给定一个正整数 n，请你求出 n 共有多少种不同的划分方法。

**输入格式**

共一行，包含一个整数 n。

**输出格式**

共一行，包含一个整数，表示总划分数量。

由于答案可能很大，输出结果请对 $10^9+7$ 取模。

**数据范围**

$1≤n≤1000$

**输入样例：**

```c
5
```

**输出样例：**

```c
7
```

**题解：**

``` c
分情况讨论：
    count(n,x):表示1~n中x出现的次数
    求[a,b]中0~9的个数：count(b,x)-count(a-1,x)
    
    eg:1~n,x=1,n=abcdefg
    分别求出1在每一位上出现的次数
    
    求1在第4位上出现的次数
    1<=xxx1yyy<=abcdefg
    (1)前三位xxx = 000~abc-1，yyy=000~999    abc*1000个
    (2)xxx=abc
        2.1 d<1 ,abc1yyy>abc0efg      0个
        2.2 d=1 ,yyy=000~efg		  efg+1个
        2.3 d>1 ,yyy=000~999          1000个
    (3)考虑边界情况
        3.1 1出现在第一位的时候，(1)不存在不用考虑，只需要
        3.2 0出现在(1)中的时候
```

**代码：**

```c

```



### 5.状态压缩DP

**[题目：AcWing 338. 计数问题]()**

**题目描述**

给定两个整数 a 和 b，求 a 和 b 之间的所有数字中 0∼9 的出现次数。

例如，a=1024，b=1032，则 a 和 b 之间共有 9 个数如下：

```
1024 1025 1026 1027 1028 1029 1030 1031 1032
```

其中 `0` 出现 10 次，`1` 出现 10 次，`2` 出现 7 次，`3` 出现 3 次等等…

**输入格式**

输入包含多组测试数据。

每组测试数据占一行，包含两个整数 a 和 b。

当读入一行为 `0 0` 时，表示输入终止，且该行不作处理。

**输出格式**

每组数据输出一个结果，每个结果占一行。

每个结果包含十个用空格隔开的数字，第一个数字表示 `0` 出现的次数，第二个数字表示 `1` 出现的次数，以此类推。

**数据范围**

$0<a,b<100000000$

**输入样例：**

```c
1 10
44 497
346 542
1199 1748
1496 1403
1004 503
1714 190
1317 854
1976 494
1001 1960
0 0
```

**输出样例：**

```c
1 2 1 1 1 1 1 1 1 1
85 185 185 185 190 96 96 96 95 93
40 40 40 93 136 82 40 40 40 40
115 666 215 215 214 205 205 154 105 106
16 113 19 20 114 20 20 19 19 16
107 105 100 101 101 197 200 200 200 200
413 1133 503 503 503 502 502 417 402 412
196 512 186 104 87 93 97 97 142 196
398 1375 398 398 405 499 499 495 488 471
294 1256 296 296 296 296 287 286 286 247
```

**题解：**

``` c

```

**代码：**

```c

```



### 6.区间DP

**[题目：AcWing 291. 蒙德里安的梦想]()**

**题目描述**

求把 N×M的棋盘分割成若干个 1×2的长方形，有多少种方案。

例如当 N=2，M=4时，共有 5 种方案。当 N=2，M=3 时，共有 3 种方案。

如下图所示：

![2411_1.jpg](https://www.acwing.com/media/article/image/2019/01/26/19_4dd1644c20-2411_1.jpg)

**输入格式**

输入包含多组测试用例。

每组测试用例占一行，包含两个整数 N 和 M。

当输入用例 N=0，M=0时，表示输入终止，且该用例无需处理。

**输出格式**

每个测试用例输出一个结果，每个结果占一行。

**数据范围**

$1≤N,M≤11$

**输入样例：**

```c
1 2
1 3
1 4
2 2
2 3
2 4
2 11
4 11
0 0
```

**输出样例：**

```c
1
0
1
2
3
5
144
51205
```

**题解：**

``` c
f(i,j)表示第i列，j存上一列伸出来的小方格，伸出来表示为1，没有出来为0，用二进制表示
    先横着存放，那么状态所对应的部分空出来的位置，一定是偶数个的格子，不然不能填充
    f(i,j)的第i列伸出来的用j表示
    f(i-1,k)的第i-1列伸出来的用k表示
    状态要能从f(i,j)转移到f(i-1,k)需要满足：
    1.j和k不能冲突(用位运算判断) j&k==0
    2.j|k的结果里面不存在连续奇数个0
```

**代码：**

```c
#include <iostream>
#include <algorithm>
#include <cstring>
using namespace std;
const int N = 12, M = 1 << N;
int n, m;
long long f[N][M];

bool st[M];

int main()
{
    while (cin >> n >> m, n || m)
    {
        memset(f, 0, sizeof f);
        for (int i = 0; i < 1 << n; i++)
        {
            //假设是成立的
            st[i] = true;
            //当前这一段连续0的个数
            int cnt = 0;

            for (int j = 0; j < n; j++)
            {
                //当前这一位是1，则表示上一段结束了
                if (i >> j & 1)
                {
                    //判断上一段0的个数是否是奇数个,不是奇数不合法
                    if (cnt & 1)
                        st[i] = false;
                    //这段结束了，重新开始清为0
                    cnt = 0;
                }
                else
                {
                    cnt++;
                }
            }
            //判断最后一段是否是1
            if (cnt & 1)
                st[i] = false;
        }
        //第一列前面没有出来的状态，是1
        f[0][0] = 1;
        //枚举所有的列
        for (int i = 1; i <= m; i++)
        {
            //枚举i列所有的状态
            for (int j = 0; j < 1 << n; j++)
            {
                //枚举第i-1列的状态
                for (int k = 0; k < 1 << n; k++)
                {
                    if ((j & k) == 0 && st[j | k])
                        f[i][j] += f[i - 1][k];
                }
            }
        }
        cout << f[m][0] << endl;
    }
    return 0;
}
```

**[题目：AcWing 91. 最短Hamilton路径]()**  

**题目描述**

给定一张 n 个点的带权无向图，点从 0∼n−1标号，求起点 0 到终点 n−1的最短 Hamilton 路径。

Hamilton 路径的定义是从 0 到 n−1不重不漏地经过每个点恰好一次。

**输入格式**

第一行输入整数 n。

接下来 n 行每行 n 个整数，其中第 i 行第 j 个整数表示点 i 到 j 的距离（记为$ a[i,j]$）。

对于任意的$ x,y,z$，数据保证 $a[x,x]=0，a[x,y]=a[y,x] $并且 $a[x,y]+a[y,z]≥a[x,z]$。

**输出格式**

输出一个整数，表示最短 Hamilton 路径的长度。

**数据范围**

$1≤n≤20$
$0≤a[i,j]≤10^7$

**输入样例：**

```c
5
0 2 4 5 1
2 0 6 5 3
4 6 0 8 3
5 5 8 0 5
1 3 3 5 0
```

**输出样例：**

```c
18
```

**题解：**

``` c
DP
    1.状态表示f(i,j)
    	(1)集合：所有从0到j,中间走过的所有点是i的所有路径，i是二进制数，每一位是表示当前点是否走过了
    	(2)属性：Min
    2.状态计算
    	倒数第二个点分类，倒数第二个点是0，1，2,.....n-1
    	(1)0---->k---->j
    	0--->k  f(i-[j],k)
    	k--->j  a(k,j)    	
```

**代码：**

```c
#include <iostream>
#include <algorithm>
#include <cstring>

using namespace std;

const int N = 20, M = 1 << N;

int n;
//存距离
int w[N][N];
int f[M][N];

int main()
{
    cin >> n;
    for (int i = 0; i < n; i++)
        for (int j = 0; j < n; j++)
            cin >> w[i][j];
    //初始化
    memset(f, 0x3f, sizeof(f));
    f[1][0] = 0;

    //枚举所有转移的状态
    for (int i = 0; i < 1 << n; i++)
        for (int j = 0; j < n; j++)
            if (i >> j & 1)
                //转移状态
                for (int k = 0; k < n; k++)
                    // i除去j这个点
                    if ((i - (1 << j)) >> k & 1)
                        f[i][j] = min(f[i][j], f[i - (1 << j)][k] + w[k][j]);
    cout << f[(1 << n) - 1][n - 1] << endl;
    return 0;
}
```



### 7.树形DP

**[题目：AcWing 285. 没有上司的舞会]()**

**题目描述**

Ural 大学有 $N$ 名职员，编号为$ 1∼N$。

他们的关系就像一棵以校长为根的树，父节点就是子节点的直接上司。

每个职员有一个快乐指数，用整数 $H_i$给出，其中 $1≤i≤N$。

现在要召开一场周年庆宴会，不过，没有职员愿意和直接上司一起参会。

在满足这个条件的前提下，主办方希望邀请一部分职员参会，使得所有参会职员的快乐指数总和最大，求这个最大值。

**输入格式**

第一行一个整数 $N$。

接下来 $N $行，第 $i $行表示 $i $号职员的快乐指数 $H_i$。

接下来 $N−1$ 行，每行输入一对整数 $L,K,$表示 $K$ 是 $L $的直接上司。

**输出格式**

输出最大的快乐指数。

**数据范围**

$1≤N≤6000,$
$−128≤H_i≤127$

**输入样例：**

```c
7
1
1
1
1
1
1
1
1 3
2 3
6 4
7 4
4 5
3 5
```

**输出样例：**

```c
5
```

**题解：**

``` c
DP
    1.状态表示 f(u,0)，f(u,1)
    	(1)集合：所有从以u为根的子树中选择，并且不选u这个点的方案f(u,0)
    			所有从以u为根的子树中选择，并且选则u这个点的方案f(u,1)
    	(2)属性：Max
    
    2.状态计算
    	u
       / \
      s1 s2
    f(u,0)  f(u,1)
    f(s1,0) f(s1,1)
    f(s2,0) f(s2,1)
    	
    f(u,0)+max(f(s1,0),f(s2,0))
    f(u,1)+Σ (f(si,0)
    
    f(u,0)=Σ max(f(si,0),f(si,1))
    f(u,1)=Σ f(si,0)
```

**代码：**

```c
#include <iostream>
#include <algorithm>
#include <cstring>

using namespace std;

const int N = 6010;
int n;
int happy[N];
int h[N], e[N], ne[N], idx;
int f[N][2];
bool has_father[N];

void add(int a, int b)
{
    e[idx] = b, ne[idx] = h[a], h[a] = idx++;
}

void dfs(int u)
{
    //选择的话加上happy
    f[u][1] = happy[u];
    //遍历儿子
    for (int i = h[u]; i != -1; i = ne[i])
    {
        int j = e[i];
        dfs(j);

        f[u][0] += max(f[j][0], f[j][1]);
        f[u][1] += f[j][0];
    }
}

int main()
{
    cin >> n;
    for (int i = 1; i <= n; i++)
        cin >> happy[i];

    memset(h, -1, sizeof(h));
    for (int i = 0; i < n - 1; i++)
    {
        int a, b;
        cin >> a >> b;
        // a的父节点为b，a标记为true
        has_father[a] = true;
        add(b, a);
    }
    int root = 1;
    while (has_father[root])
        root++;
    dfs(root);
    cout << max(f[root][0], f[root][1]);
    return 0;
}
```



### 8.数位DP

**[题目：AcWing 901. 滑雪]()**

**题目描述**

给定一个 $R $行 $C$列的矩阵，表示一个矩形网格滑雪场。

矩阵中第 $i $行第 $j $列的点表示滑雪场的第 $i$ 行第 $j$ 列区域的高度。

一个人从滑雪场中的某个区域内出发，每次可以向上下左右任意一个方向滑动一个单位距离。

当然，一个人能够滑动到某相邻区域的前提是该区域的高度低于自己目前所在区域的高度。

下面给出一个矩阵作为例子：

```
 1  2  3  4 5

16 17 18 19 6

15 24 25 20 7

14 23 22 21 8

13 12 11 10 9
```

在给定矩阵中，一条可行的滑行轨迹为 $24−17−2−1$。

在给定矩阵中，最长的滑行轨迹为$25−24−23−…−3−2−1$，沿途共经过 $25$ 个区域。

现在给定你一个二维矩阵表示滑雪场各区域的高度，请你找出在该滑雪场中能够完成的最长滑雪轨迹，并输出其长度(可经过最大区域数)。

**输入格式**

第一行包含两个整数 R 和 C。

接下来 R 行，每行包含 C 个整数，表示完整的二维矩阵。

**输出格式**

输出一个整数，表示可完成的最长滑雪长度。

**数据范围**

$1≤R,C≤300,$
$0≤矩阵中整数≤10000$

**输入样例：**

```c
5 5
1 2 3 4 5
16 17 18 19 6
15 24 25 20 7
14 23 22 21 8
13 12 11 10 9
```

**输出样例：**

```c
25
```

**题解：**

``` c
DP
    1.状态表示f(i,j)
    	(1)集合:所有从(i,j)开始滑的路径
    	(2)属性：Max
    2.状态计算
    	f(i,j)按照不同方向分为四类
    	上 (i,j)--->(i-1,j)+1
    	下 (i,j)--->(i+1,j)+1
    	左 (i,j)--->(i,j-1)+1
    	右 (i,j)--->(i,j+1)+1
```

**代码：**

```c
#include <iostream>
#include <algorithm>
#include <cstring>

using namespace std;
const int N = 310;
int n, m;
//表示高度
int h[N][N];
//状态
int f[N][N];

int dx[4] = {-1, 0, 1, 0}, dy[4] = {0, 1, 0 - 1};

int dp(int x, int y)
{
    //表示状态,状态已经算过则直接返回
    int &v = f[x][y];
    if (v != -1)
        return v;

    // 否则算一下，先初始v最少为1
    v = 1;
    for (int i = 0; i < 4; i++)
    {
        int a = x + dx[i], b = y + dy[i];
        if (a >= 1 && a <= n && b >= 1 && b <= m && h[a][b] < h[x][y])
            v = max(v, dp(a, b) + 1);
    }
    return v;
}

int main()
{
    cin >> n >> m;
    for (int i = 1; i <= n; i++)
        for (int j = 1; j <= m; j++)
            cin >> h[i][j];
    //-1表示没有被算过
    memset(f, -1, sizeof(f));

    int res = 0;
    for (int i = 1; i <= n; i++)
        for (int j = 1; j <= m; j++)
            res = max(res, dp(i, j));
    cout << res << endl;
    return 0;
}
```

### 9.单调队列优化DP



### 10.斜率优化DP



## 二.贪心

### 1.区间问题

[题目：AcWing 905. 区间选点]()

**题目描述**

给定N个闭区间[ai,bi]，请你在数轴上选择尽量少的点，使得每个区间内至少包含一个选出的点。

输出选择的点的最小数量。

位于区间端点上的点也算作区间内。

**输入格式**

第一行包含整数N，表示区间数。

接下来N行，每行包含两个整数ai,bi，表示一个区间的两个端点。

**输出格式**

输出一个整数，表示所需的点的最小数量。

**数据范围**

$1≤N≤10^5,$

$−10^9≤ai≤bi≤10^9$

**输入样例：**

```c
3
-1 1
2 4
3 5
```

**输出样例：**

```c
2
```

**题解：**

``` c
1.将每个区间按照右端点从小到大排序
2.从前往后依次枚举每一个区间（单峰的时候可以用贪心）
    如果当前区间已经包含点，则直接pass
    否则，选择当前区间的右端点
3.可行解里面的最小值ans<=当前满足的可行解cnt
4.ans>=cnt
5.ans=cnt
```

**代码：**

```c
#include <iostream>
#include <algorithm>
#include <string>

using namespace std;

const int N = 100010;

int n;
//按照右区间排序
struct Range
{
    int l, r;
    bool operator<(const Range &W) const
    {
        return r < W.r;
    }
} range[N];

int main()
{
    cin >> n;
    for (int i = 0; i < n; i++)
    {
        int l, r;
        cin >> l >> r;
        range[i] = {l, r};
    }
    sort(range, range + n);
    // res表示当前选择点的数量，ed表示上一个点的下标
    int res = 0, ed = -2e9;
    for (int i = 0; i < n; i++)
    {
        if (range[i].l > ed)
        {
            res++;
            //优先选择右端点
            ed = range[i].r;
        }
    }
    cout << res << endl;
    return 0;
}
```



**[题目：AcWing 908. 最大不相交区间数量]()**

**题目描述**

给定N个闭区间[ai,bi]，请你在数轴上选择若干区间，使得选中的区间之间互不相交（包括端点）。

输出可选取区间的最大数量。

**输入格式**

第一行包含整数N，表示区间数。

接下来N行，每行包含两个整数ai,bi，表示一个区间的两个端点。

**输出格式**

输出一个整数，表示可选取区间的最大数量。

**数据范围**

$1≤N≤10^5,$

$−10^9≤ai≤bi≤10^9$

**输入样例：**

```c
3
-1 1
2 4
3 5
```

**输出样例：**

```c
2
```

**题解：**

``` c
1.将每个区间按照右端点从小到大排序
2.从前往后依次枚举每一个区间（单峰的时候可以用贪心）
    当上一个区间的r和当前区间l没有交集的时候才能选择
    
3.所有可行方案的最大值ans>=当前的可行方案cnt
4.ans<=cnt
    反证法：
    假设ans>cnt,可以选择cnt更多的ans答案，因为当前已经选择了cnt个点，如果有大于ans的点，所以需要选择ans个点才能覆盖区间，实际cnt个点就可以覆盖了，则矛盾了
    
5.ans=cnt
```

**代码：**

```c
#include <iostream>
#include <algorithm>

using namespace std;

const int N = 100010;

struct Range
{
    int l, r;
    bool operator<(const Range &W) const
    {
        return r < W.r;
    }
} range[N];
int n;

int main()
{
    cin >> n;
    for (int i = 0; i < n; i++)
    {
        int l, r;
        cin >> l >> r;
        range[i] = {l, r};
    }
    sort(range, range + n);
    // res表示当前选择点的数量，ed表示上一个点的下标
    int res = 0, ed = -2e9;
    for (int i = 0; i < n; i++)
    {
        //当两个区间不相交的时候
        if (ed < range[i].l)
        {
            res++;
            ed = range[i].r;
        }
    }
    cout << res << endl;
    return 0;
}
```



**[题目：AcWing 906. 区间分组]()**

**题目描述**

给定 N 个闭区间$ [a_i,b_i]，$请你将这些区间分成若干组，使得每组内部的区间两两之间（包括端点）没有交集，并使得组数尽可能小。

输出最小组数。

**输入格式**

第一行包含整数 N，表示区间数。

接下来 N 行，每行包含两个整数 $a_i,b_i$，表示一个区间的两个端点。

**输出格式**

输出一个整数，表示最小组数。

**数据范围**

$1≤N≤105$,

$−10^9≤ai≤bi≤10^9$

**输入样例：**

```c
3
-1 1
2 4
3 5
```

**输出样例：**

```c
2
```

**题解：**

``` c
1.将每个区间按照左端点从小到大排序
    
2.从前往后依次枚举每一个区间（单峰的时候可以用贪心）
    判断能否将其放到某个现有的组中l>max_r:
        (1)如果不存在这样的组，当前区间左端点l是否小于某个组的max_r
            则当前区间是和前面的都冲突，则开一个新的组，然后再将其放进去
        (2)如果存在这样的组，l>max_r
            随便选一个将其放进去，并且更新当前组的max_r

3.证明：
	(1)ans<=cnt
    (2)ans>=cnt
    (3)ans==cnt
```

**代码：**

```c
#include <iostream>
#include <algorithm>
#include <queue>

using namespace std;
const int N = 100010;
int n;
struct Range
{
    int l, r;
    bool operator<(const Range &W) const
    {
        return l < W.l;
    }
} range[N];

int main()
{
    cin >> n;
    for (int i = 0; i < n; i++)
    {
        int l, r;
        cin >> l >> r;
        range[i] = {l, r};
    }
    sort(range, range + n);
    //定义小根堆
    priority_queue<int, vector<int>, greater<int>> heap;
    for (int i = 0; i < n; i++)
    {
        auto r = range[i];
        //如果小根堆里面没有或者如果不存在这样的组
        //则当前区间是和前面的都冲突，则开一个新的组，然后再将其放进去
        if (heap.empty() || heap.top() >= r.l)
            heap.push(r.r);
        //如果存在这样的组，l>max_r
        //随便选一个将其放进去，并且更新当前组的max_r
        else
        {
            //放到最小值里面的组
            int t = heap.top();
            heap.pop();
            heap.push(r.r);
        }
    }
    cout << heap.size() << endl;
    return 0;
}
```



**[题目：AcWing 907. 区间覆盖]()**

**题目描述**

给定 N 个闭区间$ [a_i,b_i]$ 以及一个线段区间 $[s,t]$，请你选择尽量少的区间，将指定线段区间完全覆盖。

输出最少区间数，如果无法完全覆盖则输出 −1。

**输入格式**

第一行包含两个整数 $s $和 $t$，表示给定线段区间的两个端点。

第二行包含整数$ N$，表示给定区间数。

接下来$ N$ 行，每行包含两个整数 $a_i,b_i$，表示一个区间的两个端点。

**输出格式**

输出一个整数，表示所需最少区间数。

如果无解，则输出 −1。

**数据范围**

$1≤N≤10^5$,
$−10^9≤a_i≤b_i≤10^9$,
$−10^9≤s≤t≤10^9$

**输入样例：**

```c
1 5
3
-1 3
2 4
3 5
```

**输出样例：**

```c
2
```

**题解：**

``` c
	1-----------5
   start		end
1.将所有的区间按照左端点从小到大排序
2.从前往后依次枚举每个区间，在所有能覆盖start的区间当中，选择右端点最大的区间
  然后将start更新成右端点的最大值
3.证明：ans最优解，cnt可行解
	(1)ans<=cnt
    (2)ans>=cnt
        (√)将任何一个最优解通过调整法得到
        (x)反证法：
        假设存在一个方案  ans<cnt
        将ans和cnt的左端点排序，从前往后找到ans和cnt中第一个不相同的区间，可以将ans的区间变为cnt的区间的;后面同样的道理，可以将ans后面的区间都替换为cnt的区间
    (3)ans==cnt
```

**代码：**

```c
#include <iostream>
#include <algorithm>

using namespace std;

const int N = 100010;
int n;

struct Range
{
    int l, r;
    bool operator<(const Range &W) const
    {
        return l < W.l;
    }
} range[N];
int main()
{
    //目标区间
    int st, ed;
    cin >> st >> ed;
    cin >> n;
    for (int i = 0; i < n; i++)
    {
        int l, r;
        cin >> l >> r;
        range[i] = {l, r};
    }
    int res = 0;
    sort(range, range + n);
    bool success = false; // 找出了方案
    for (int i = 0; i < n; i++)
    {
        // r表示当前最大值,选择满足左端点，右端点尽量大
        int j = i, r = -2e9;
        //遍历所有左端点在st左边，r最大的区间
        while (j < n && range[j].l <= st)
        {
            r = max(r, range[j].r);
            j++;
        }
        if (r < st)
        {
            res = -1;
            break;
        }
        res++;
        if (r >= ed)
        {
            success = true;
            break;
        }
        st = r;
        i = j - 1;
    }

    if (!success)
        res = -1;
    cout << res << endl;
    return 0;
}
```

### 2.Huffman数

**[题目：AcWing 148. 合并果子]()**

**题目描述**

在一个果园里，达达已经将所有的果子打了下来，而且按果子的不同种类分成了不同的堆。

达达决定把所有的果子合成一堆。

每一次合并，达达可以把两堆果子合并到一起，消耗的体力等于两堆果子的重量之和。

可以看出，所有的果子经过 $n−1$ 次合并之后，就只剩下一堆了。

达达在合并果子时总共消耗的体力等于每次合并所耗体力之和。

因为还要花大力气把这些果子搬回家，所以达达在合并果子时要尽可能地节省体力。

假定每个果子重量都为 1，并且已知果子的种类数和每种果子的数目，你的任务是设计出合并的次序方案，使达达耗费的体力最少，并输出这个最小的体力耗费值。

例如有 3 种果子，数目依次为 $1，2，9$。

可以先将 $1、2$ 堆合并，新堆数目为 3，耗费体力为 3。

接着，将新堆与原先的第三堆合并，又得到新的堆，数目为 12，耗费体力为 12。

所以达达总共耗费体力=3+12=15。

可以证明 15 为最小的体力耗费值。

**输入格式**

输入包括两行，第一行是一个整数 n，表示果子的种类数。

第二行包含 n 个整数，用空格分隔，第 i 个整数 $a_i$ 是第 i 种果子的数目。

**输出格式**

输出包括一行，这一行只包含一个整数，也就是最小的体力耗费值。

输入数据保证这个值小于$2^31$。

**数据范围**

$1≤n≤10000$,
$1≤a_i≤20000$

**输入样例：**

```c
3 
1 2 9 
```

**输出样例：**

```c
15
```

**题解：**

``` c
 1.所有的树里面，权值最小的两个点，深度一定最深，并且可以互为兄弟
```

**代码：**

```c
#include <iostream>
#include <algorithm>
#include <queue>

using namespace std;

int main()
{
    int n;
    cin >> n;
    priority_queue<int, vector<int>, greater<int>> heap;
    while (n--)
    {
        int x;
        cin >> x;
        heap.push(x);
    }
    int res = 0;
    while (heap.size() > 1)
    {
        int a = heap.top();
        heap.pop();
        int b = heap.top();
        heap.pop();
        res += a + b;
        heap.push(a + b);
    }
    cout << res << endl;
    return 0;
}
```



### 3. 排列不等式

**[题目：AcWing 913. 排队打水]()**

**题目描述**

有 n 个人排队到 1 个水龙头处打水，第 i 个人装满水桶所需的时间是 $t_i$，请问如何安排他们的打水顺序才能使所有人的等待时间之和最小？

**输入格式**

第一行包含整数 n。

第二行包含 n 个整数，其中第 i 个整数表示第 i 个人装满水桶所花费的时间 $t_i$。

**输出格式**

输出一个整数，表示最小的等待时间之和。

**数据范围**

$1≤n≤10^5,$
$1≤t_i≤10^4$

**输入样例：**

```c
7
3 6 1 4 2 5 7
```

**输出样例：**

```c
56
```

**题解：**

``` c
总时间=t1*(n-1)+t2*(n-2)+t3*(n-3)......
按照从小到大的顺序排队，总时间最小
证明：
如果t[i]>t[i+1]
	交换两个数的位置
    交换前：t[i]*(n-i)+t[i+1]*(n-i-1)
    交换后：t[i+1]*(n-i)+t[i]*(n-i-1)
    交换前-交换后=t[i]-t[i+1]>0
    
```

**代码：**

```c
#include <iostream>
#include <algorithm>

typedef long long ll;
using namespace std;
const int N = 100010;

int n;
int t[N];

int main()
{
    cin >> n;
    for (int i = 0; i < n; i++)
        cin >> t[i];

    sort(t, t + n);
    ll res = 0;
    for (int i = 0; i < n; i++)
        res += t[i] * (n - i - 1);
    cout << res << endl;
    return 0;
}
```



### 4. 绝对值不等式

**[题目：AcWing 104. 货仓选址]()**

**题目描述**

在一条数轴上有 N 家商店，它们的坐标分别为 $A_1∼A_N$。

现在需要在数轴上建立一家货仓，每天清晨，从货仓到每家商店都要运送一车商品。

为了提高效率，求把货仓建在何处，可以使得货仓到每家商店的距离之和最小。

**输入格式**

第一行输入整数 N。

第二行 N 个整数 $A_1∼A_N$。

**输出格式**

输出一个整数，表示距离之和的最小值。

**数据范围**

$1≤N≤100000$,
$0≤A_i≤40000$

**输入样例：**

```c
4
6 2 9 1
```

**输出样例：**

```c
12
```

**题解：**

``` c
把仓库位置记为x
    f(x)=|x1-x|+|x2-x|+|x3-x|+.....+|xn-x|
    x为中位数
    当N为奇数的时候是中间，为偶数的时候两个数任意一个都可以
    f(x)=(|x1-x|+|xn-x|)+(|x2-x|+|xn-1-x|)+....
    	>=(xn-x1)+(xn-1-x2)+....
```

**代码：**

```c
#include <iostream>
#include <algorithm>
using namespace std;

const int N = 100010;

int n;
int a[N];

int main()
{
    cin >> n;
    for (int i = 0; i < n; i++)
        cin >> a[i];
    sort(a, a + n);
    int res = 0;

    for (int i = 0; i < n; i++)
        res += abs(a[i] - a[n / 2]);
    cout << res << endl;
    return 0;
}
```



### 5. 推公式

**[题目：AcWing 125. 耍杂技的牛]()**

**题目描述**

农民约翰的 N 头奶牛（编号为 $1..N$）计划逃跑并加入马戏团，为此它们决定练习表演杂技。

奶牛们不是非常有创意，只提出了一个杂技表演：

叠罗汉，表演时，奶牛们站在彼此的身上，形成一个高高的垂直堆叠。

奶牛们正在试图找到自己在这个堆叠中应该所处的位置顺序。

这 N 头奶牛中的每一头都有着自己的重量 $W_i$ 以及自己的强壮程度 $S_i$。

一头牛支撑不住的可能性取决于它头上所有牛的总重量（不包括它自己）减去它的身体强壮程度的值，现在称该数值为风险值，风险值越大，这只牛撑不住的可能性越高。

您的任务是确定奶牛的排序，使得所有奶牛的风险值中的最大值尽可能的小。

**输入格式**

第一行输入整数 N，表示奶牛数量。

接下来 N 行，每行输入两个整数，表示牛的重量和强壮程度，第 i 行表示第 i 头牛的重量 $W_i$ 以及它的强壮程度 $S_i$。

**出格式**

输出一个整数，表示最大风险值的最小可能值。

**数据范围**

$1≤N≤50000$,
$1≤W_i≤10,000,$
$1≤S_i≤1,000,000,000$

**输入样例：**

```c
3
10 3
2 5
3 3
```

**输出样例：**

```c
2
```

**题解：**

``` c
按照wi+si从小到大的顺序排序，最大的危险系数一定是最小的
证明：
    1.贪心得到的答案一定是>=最优解
    2.贪心得到的答案一定是<=最优解
    	wi+si>w(i+1)+s(i+1)
    (1)交换第i和i+1的牛:
    			第i个位置的牛     	第i+1个位置的牛
  	交换前	 w1+w2+...+w(i-1)-si   w1+...+wi-s(i+1)
    交换后  w1+...w(i-1)-s(i+1)   w1+...+w(i-1)+w(i+1)-si
    (2)去掉公共部分的w1+w2+...+w(i-1)
                第i个位置的牛     	第i+1个位置的牛
  	交换前	 		-si   				wi-s(i+1)
    交换后  		-s(i+1)   			w(i+1)-si
    (3)同时加+si+s(i+1)
                 第i个位置的牛     	第i+1个位置的牛
  	交换前	 	 -si+si+s(i+1)   		wi-s(i+1)+si+s(i+1)
    交换后  	-s(i+1)+si+s(i+1)   	w(i+1)-si+si+s(i+1)
    (4)得到结果
                 第i个位置的牛     	第i+1个位置的牛
  	交换前	 	 	s(i+1)   			wi+si
    交换后  		 si  			 w(i+1)+s(i+1)
                   
    所以只要存在逆序wi+si>w(i+1)+s(i+1)则交换之后不会变大，小于等于最优解
```

**代码：**

```c
#include <iostream>
#include <algorithm>
#include <cstring>
using namespace std;
typedef pair<int, int> PII;

const int N = 50010;

int n;
PII cow[N];

int main()
{
    cin >> n;
    for (int i = 0; i < n; i++)
    {
        int w, s;
        cin >> w >> s;
        cow[i] = {w + s, w};
    }
    sort(cow, cow + n);
    // sum表示每头牛上面的w总和
    int res = -2e9, sum = 0;
    for (int i = 0; i < n; i++)
    {
        int w = cow[i].second, s = cow[i].first - w;
        res = max(res, sum - s);
        sum += w;
    }
    cout << res << endl;
    return 0;
}
```



## 二.搜索

### BFS

应用：

①最短距离

②最小步数

特点：

①求最小，最短

②基于迭代的思想，不会爆栈

### 1.Flood Fill

**在线性时间复杂度里面，找到某个点所在的连通块**

#### 1.1  池塘计数（八连通BFS）

**[题目：AcWing1097. 池塘计数]()**

**题目描述**

农夫约翰有一片 N∗M 的矩形土地。

最近，由于降雨的原因，部分土地被水淹没了。

现在用一个字符矩阵来表示他的土地。

每个单元格内，如果包含雨水，则用”W”表示，如果不含雨水，则用”.”表示。

现在，约翰想知道他的土地中形成了多少片池塘。

每组相连的积水单元格集合可以看作是一片池塘。

每个单元格视为与其上、下、左、右、左上、右上、左下、右下八个邻近单元格相连。

请你输出共有多少片池塘，即矩阵中共有多少片相连的”W”块。

**输入格式**

第一行包含两个整数 N 和 M。

接下来 N行，每行包含 M个字符，字符为”W”或”.”，用以表示矩形土地的积水状况，字符之间没有空格。

**输出格式**

输出一个整数，表示池塘数目。

**数据范围**

1≤N,M≤1000	

**输入样例：**

```c
10 12
W........WW.
.WWW.....WWW
....WW...WW.
.........WW.
.........W..
..W......W..
.W.W.....WW.
W.W.W.....W.
.W.W......W.
..W.......W.
```

**输出样例：**

```c
3
```

**题解：**

``` c

```

**代码：**

```c
#include <cstring>
#include <iostream>
#include <algorithm>

#define x first
#define y second

using namespace std;

typedef pair<int, int> PII;

const int N = 1010, M = N * N;

int n, m;
char g[N][N];
PII q[M];
bool st[N][N];	//bfs很多时候都需要开标记数组

void bfs(int sx, int sy)
{
    int hh = 0, tt = 0;
    q[0] = {sx, sy};
    st[sx][sy] = true;

    while (hh <= tt)
    {
        PII t = q[hh ++ ];

        for (int i = t.x - 1; i <= t.x + 1; i ++ )
            for (int j = t.y - 1; j <= t.y + 1; j ++ )
            {
                if (i == t.x && j == t.y) continue;
                //判重，越界，撞墙
                if (i < 0 || i >= n || j < 0 || j >= m) continue;
                if (g[i][j] == '.' || st[i][j]) continue;
                q[ ++ tt] = {i, j};
                st[i][j] = true;
            }
    }
}

int main()
{
    scanf("%d%d", &n, &m);
    for (int i = 0; i < n; i ++ ) scanf("%s", g[i]);

    int cnt = 0;
    for (int i = 0; i < n; i ++ )
        for (int j = 0; j < m; j ++ )
            if (g[i][j] == 'W' && !st[i][j])
            {
                bfs(i, j);
                cnt ++ ;
            }

    printf("%d\n", cnt);

    return 0;
}
```



### 2、



## 五.树状数组、线段树

### 1、线段树

### **Acwing1275最大数**    

给定一个正整数数列 $a_1,a_2,…,a_n$，每一个数都在 `0∼p−1`之间。

可以对这列数进行两种操作：

1. 添加操作：向序列后添加一个数，序列长度变成 `n+1`；
2. 询问操作：询问这个序列中最后 `L`个数中最大的数是多少。

程序运行的最开始，整数序列为空。

一共要对整数序列进行 `m`次操作。

写一个程序，读入操作的序列，并输出询问操作的答案。

#### 输入格式

第一行有两个正整数 `m,p`，意义如题目描述；

接下来 `m`行，每一行表示一个操作。

如果该行的内容是 `Q L`，则表示这个操作是询问序列中最后 `L` 个数的最大数是多少；

如果是 `A t`，则表示向序列后面加一个数，加入的数是`(t+a) mod p`。其中，`t` 是输入的参数，`a`是在这个添加操作之前最后一个询问操作的答案（如果之前没有询问操作，则 `a=0`）。

第一个操作一定是添加操作。对于询问操作，`L>0` 且不超过当前序列的长度。

#### 输出格式

对于每一个询问操作，输出一行。该行只有一个数，即序列中最后 `L`个数的最大数。

#### 数据范围

$1≤m≤2×10^5$,
$1≤p≤2×10^9$,
$0≤t<p$

#### 输入样例：

```
10 100
A 97
Q 1
Q 1
A 17
Q 2
A 63
Q 1
Q 1
Q 3
A 99
```

#### 输出样例：

```
97
97
97
60
60
97
```

#### 样例解释

最后的序列是 97,14,60,96。

**c++代码实现：**

![image-20230315164557036](../../../zky/Coding/Coderwhy/pic/image-20230315164557036.png)

![image-20230315164624686](../../../zky/Coding/Coderwhy/pic/image-20230315164624686.png)

![image-20230315164641441](../../../zky/Coding/Coderwhy/pic/image-20230315164641441.png)

```c
#include <cstdio>
#include <algorithm>
#include <string>
#include <iostream>

#define ac cin.tie(0), cin.sync_with_stdio(0);
#define endl "\n"

using namespace std;

const int N = 2e5+10;

int m,p;

struct Node{
    int l,r;
    int v;  //携带的信息，这里是[l,r]区间的最大值
}tr[N*4];

//pushup操作，由子节点的信息，来计算父节点的信息
void pushup(int u){
    tr[u].v=max(tr[u<<1].v,tr[u<<1|1].v);
}

void build(int u,int l,int r){
    // tr[u]={l,r};
    tr[u].l=l,tr[u].r=r;    //节点u代表区间[l,r]
    if(l==r) return;    //是叶子节点
    int mid=l+r>>1;
    build(u<<1,l,mid),build(u<<1|1,mid+1,r);
}

int query(int u,int l,int r){
    if(tr[u].l>=l && tr[u].r<=r) return tr[u].v;    //查询在区间[l,r]的最大值，包含当前的节点u，则直接回溯返回当前点的值
    int mid=tr[u].l+tr[u].r>>1;
    int v=0;
    // if(l<=mid) v=max(v,query(u<<1,l,r));
    if(l<=mid) v=query(u<<1,l,r);
    if(r>mid) v=max(v,query(u<<1|1,l,r));
    // else v=max(v,query(u<<1|1,l,r));
    return v;
}

void modify(int u,int x,int v){
    if(tr[u].l==x && tr[u].r==x) tr[u].v=v;
    else{
        int mid=tr[u].l+tr[u].r>>1;
        if(x<=mid) modify(u<<1,x,v);
        else modify(u<<1|1,x,v);
        pushup(u);
    }
}

int main(){
    int n=0,last=0;   //last记录题目中所说的上次查询的结果a
    scanf("%d%d",&m,&p);
    build(1,1,m);
    
    int x;
    char op[2];
    while(m--){
        scanf("%s%d",op,&x);
        if(*op=='Q'){
            last=query(1,n-x+1,n);
            printf("%d\n",last);
        }else{
            modify(1,n+1,((long long)last+x)%p);
            n++;
        }
    }
    return 0;
}
```





## 7.由数据范围反推算法复杂度以及算法内容

一般ACM或者笔试题的时间限制是1秒或2秒。
在这种情况下，$C++$代码中的操作次数控制在 $10^7$ ~$10^8$为最佳。

下面给出在不同数据范围下，代码的时间复杂度和算法该如何选择：

1. $n≤30$, 指数级别,$ dfs+$剪枝，状态压缩$dp$
2. $n≤100 => O(n^3)$，$floyd$，$dp$，高斯消元
3. $n≤1000 => O(n^2)，O(n^2logn)$，$dp$，二分，朴素版$Dijkstra$、朴素版$Prim$、$Bellman-Ford$
4. $n≤10000 => O(n∗\sqrt{n})$，块状链表、分块、莫队
5. $n≤100000 => O(nlogn)$ => 各种$sort$，线段树、树状数组、$set/map$、$heap$、拓扑排序、$dijkstra+heap$、 $prim+heap$、$Kruskal$、$spfa$、求凸包、求半平面交、二分、CDQ分治、整体二分、后缀数组、树链剖分、动态树
6. $n≤1000000 => O(n)$, 以及常数较小的$ O(nlogn)$ 算法 => 单调队列、$hash$、双指针扫描、并查集，$kmp$、$AC$自动机，常数比较小的$ O(nlogn)$的做法：$sort$、树状数组、$heap$、$dijkstra$、$spfa$
7. $n≤10000000 => O(n)$，双指针扫描、$kmp$、$AC$自动机、线性筛素数
8. $n≤10^9 => O(\sqrt{n})$，判断质数
9. $n≤10^{18} => O(logn)$，最大公约数，快速幂，数位$DP$
10. $n≤10^{1000} => O((logn)^2)$，高精度加减乘除
11. $n≤10^{100000} => O(logn×loglogn)$，k表示位数 ，高精度加减、$FFT/NTT$

空间分析

1 Byte = 8 bit

1KB =  1024 Byte

1MB = 1024 * 1024 Byte

1GB = 1024 * 1024 * 1024 Byte

int 4 Byte

char 1 Byte

double,long long 8 Byte

 64MB=2^26 Byte

2^26 /4=2^24,16000000







