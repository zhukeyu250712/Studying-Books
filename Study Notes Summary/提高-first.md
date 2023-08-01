## 四、高级数据结构

### 1、并查集



### 2、树状数组



### 3、线段树

#### (1) pushup

####  3.1 AcWing 1275.最大数    

**[题目：AcWing 1275.最大数   ]()**

**题目描述**

给定一个正整数数列 $a_1,a_2,…,a_n$，每一个数都在 `0∼p−1`之间。

可以对这列数进行两种操作：

1. 添加操作：向序列后添加一个数，序列长度变成 `n+1`；
2. 询问操作：询问这个序列中最后 `L`个数中最大的数是多少。

程序运行的最开始，整数序列为空。

一共要对整数序列进行 `m`次操作。

写一个程序，读入操作的序列，并输出询问操作的答案。

**输入格式**

第一行有两个正整数 `m,p`，意义如题目描述；

接下来 `m`行，每一行表示一个操作。

如果该行的内容是 `Q L`，则表示这个操作是询问序列中最后 `L` 个数的最大数是多少；

如果是 `A t`，则表示向序列后面加一个数，加入的数是`(t+a) mod p`。其中，`t` 是输入的参数，`a`是在这个添加操作之前最后一个询问操作的答案（如果之前没有询问操作，则 `a=0`）。

第一个操作一定是添加操作。对于询问操作，`L>0` 且不超过当前序列的长度。

**输出格式**

对于每一个询问操作，输出一行。该行只有一个数，即序列中最后 `L`个数的最大数。

**数据范围**

$1≤m≤2×10^5$,
$1≤p≤2×10^9$,
$0≤t<p$

**输入样例：**

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

**输出样例：**

```
97
97
97
60
60
97
```

**样例解释**

最后的序列是 97,14,60,96。

**c++代码实现：**

```
线段树
1.线段树的每个节点都代表一个区间。
2.线段树具有唯一的根结点，代表的区间是整个统计范围，如[1,N]。
3.线段树的每个叶结点都代表一个长度为1的元区间[x,x]。
4.对于每个内部结点[l,r],他的左子结点是[l,mid],右子结点是[mid十1,r],其中mid=(l+r)>>1

线段树Tip
1.线段树是一棵非常漂亮二叉树，(除了最后一层外，是一棵满二叉树)，因此我们采用堆的思想来存整棵树
(1) 编号x的父节点：x/2 , 常书写的代码：x>>1
(2) 编号x的左儿子：2x , 常书写的代码：x<<1
(3) 编号x的右儿子：2x十1，常书写的代码：×<<1|1
2线段的下一层都是把当前层进行平分mid=L l+r>>1」
(1) 左区间为[l，mid]
(2) 右区间为[mid+1,r]
注意，线段树的两个子区间是不允许相交的,这也决定了许多题的分块要进行额外的操作，使之区间不能相交
3.线段树一般开长度为4N的空间
(1) 一个长度为N的区间，最终的叶结点，为N个
(2) 先考虑理想状态下(包含最后一层，整个二叉树都是满二叉树)，N个叶结点的满二叉树有
N+N/2+N/4+.....+2+1=2N-1个结点
(3) 但是线段树的存储方式下，最后一层还会有盈余，最坏情况下，最后一层是倒数第二层（也就是满二叉树的倒数第一层）两倍的点，所以所以保存线段树的数组长度要不小于4N才能保证不会越界

线段树的模版
1,pushup（）:由子结点的信息来计算父结点的信息
2.bui1d（）:将一段区间初始化为线段树
3.modify（）:修改
(1) 单点修改(easy)
(2) 区间修改(hard)：用到pushdown操作，懒标记思想
4.query（）:区间询问(例如：查间[a,b]区间)O(1ogn)最多是4logn的时间，因为最坏有两条链
```

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

//build的时候先赋值避免忘记
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

Q:线段树建树build操作的时候，什么时候需要加pushup，什么时候不加pushup

A：在线段树的建树过程中，通常需要执行pushup操作来更新父节点的信息，以确保它的信息与子节点保持一致。pushup操作的时机取决于当前节点的性质以及子节点的性质，具体情况如下：

1. 对于区间修改操作（如单点修改、区间加、区间赋值等），因为修改操作会改变子节点的值，所以在递归调用build函数时需要先对子节点进行更新，再对父节点进行pushup操作。
2. 对于区间查询操作，因为查询操作只需要使用子节点的信息计算父节点的信息，而不需要修改子节点的信息，所以在递归调用build函数时不需要进行pushup操作。
3. 对于区间合并操作（如区间求和、区间最大值、区间最小值等），由于子节点的信息可以通过简单的合并得到父节点的信息，因此在递归调用build函数时需要先对子节点进行更新，再对父节点进行pushup操作。

总之，需要加pushup的情况是在对子节点进行更新之后，需要确保父节点的信息与子节点保持一致。不需要加pushup的情况是在不需要修改子节点的信息的情况下进行递归调用。

[推荐博客](https://www.cnblogs.com/zheyuanxie/archive/2022/08/09/segment-tree.html)

​        

#### 3.2 AcWing 245. 你能回答这些问题吗      

**[题目：AcWing 245. 你能回答这些问题吗  ]()**

**题目描述**

给定长度为 *N* 的数列 *A*，以及 *M*

 条指令，每条指令可能是以下两种之一：

1. `1 x y`，查询区间 [*x*,*y*]中的最大连续子段和，即$ max_{x≤l≤r≤y}{∑_{i=l}^rA[i]}$。

2. `2 x y`，把 *A*[*x*]改成 *y*

对于每个查询指令，输出一个整数表示答案。

**输入格式**

第一行两个整数 *N*,*M*。

第二行 *N*个整数 *A*[*i*]。

接下来 *M*行每行 3 个整数 *k*,*x*,*y*，*k*=1 表示查询（此时如果 *x*>*y*，请交换 *x*,*y*），*k*=2 表示修改。

**输出格式**

对于每个查询指令输出一个整数表示答案。

每个答案占一行。

**数据范围**

$N≤500000≤M≤100000$,
 $−1000≤A[i]≤1000$,

**输入样例：**

```r
5 3
1 2 -3 4 5
1 2 3
2 2 -1
1 3 2
```

**输出样例：**

```
2
-1
```

**题解：**

``` c
struct Node{
  int l,r;  //区间左右端点
  int sum; //区间和
  int lmax; //最大前缀和
  int rmax; //最大后缀和
  int tmax; //最大连续子段和
  //横跨左右子区间的最大子段和 = 左子区间的最大后缀 + 右子区间的最大前缀
}tr[N*4];

void pushup(Node& u,Node& l,Node& r){
  u.sum=l.sum+r.sum;  //区间和=左区间和+右区间和
  //最大前缀和:左儿子的前缀和（只有左儿子前缀）、左儿子区间和+右儿子的前缀和（覆盖到右儿子）
  u.lmax=max(l.lmax,l.sum+r.lmax);  
  //最大后缀和:右儿子的后缀和（只有右儿子后缀）、左儿子后缀+右儿子的区间和（覆盖到右儿子）
  u.rmax=max(r.rmax,l.rmax+r.sum);  
  //当只有左边或者右边儿子、跨越两个区间左儿子取后缀右儿子取前缀
  u.tmax=max(max(l.tmax,r.tmax),l.rmax+r.lmax);
}
```

**代码：**

```c
#include <cstdio>
#include <algorithm>
#include <cstring>
#include <iostream>

using namespace std;
const int N = 5e5+10;
int w[N];
int n,m;

struct Node{
  int l,r;  //区间左右端点
  int sum; //区间和
  int lmax; //最大前缀和
  int rmax; //最大后缀和
  int tmax; //最大连续子段和
  //横跨左右子区间的最大子段和 = 左子区间的最大后缀 + 右子区间的最大前缀
}tr[N*4];

void pushup(Node& u,Node& l,Node& r){
  u.sum=l.sum+r.sum;  //区间和=左区间和+右区间和
  //最大前缀和:左儿子的前缀和（只有左儿子前缀）、左儿子区间和+右儿子的前缀和（覆盖到右儿子）
  u.lmax=max(l.lmax,l.sum+r.lmax);  
  //最大后缀和:右儿子的后缀和（只有右儿子后缀）、左儿子后缀+右儿子的区间和（覆盖到右儿子）
  u.rmax=max(r.rmax,l.rmax+r.sum);  
  //当只有左边或者右边儿子、跨越两个区间左儿子取后缀右儿子取前缀
  u.tmax=max(max(l.tmax,r.tmax),l.rmax+r.lmax);
}

void pushup(int u){
  pushup(tr[u],tr[u<<1],tr[u<<1|1]);
}

void build(int u,int l,int r){
  tr[u].l=l,tr[u].r=r;	//	容易忘记写在else里面导致段错误
  //如果是叶子节点
  if(l==r){
    tr[u].sum=w[l];
    tr[u].lmax=w[l];
    tr[u].rmax=w[l];
    tr[u].tmax=w[l];
  }else{
    int mid=l+r>>1;
    build(u<<1,l,mid);
    build(u<<1|1,mid+1,r);
    pushup(u);
  }
}

void modify(int u,int x,int v){
  if(tr[u].l==x && tr[u].r==x){
    tr[u].l=x, tr[u].r=x, tr[u].sum=v, tr[u].lmax=v, tr[u].rmax=v, tr[u].tmax=v;
  }else{
    int mid=tr[u].l + tr[u].r>>1;
    if(x<=mid) modify(u<<1,x,v);
    else modify(u<<1|1,x,v);
    pushup(u);
  }
}

Node query(int u,int l,int r){
  if(tr[u].l>=l && tr[u].r<=r) return tr[u];
  else{
    int mid=tr[u].l+tr[u].r>>1;
    /*
    1、在左区间  r<=mid
    2、在右区间  l>mid
    3、左区间+右区间
    */
    if(r<=mid) return query(u<<1,l,r);
    else if(l>mid) return query(u<<1|1,l,r);
    else{
      Node left=query(u<<1,l,r);
      Node right=query(u<<1|1,l,r);
      Node res;
      pushup(res,left,right);
      return res;
    }
  }
}

int main()
{
  scanf("%d%d",&n,&m);
  //数组下标从1开始
  for(int i=1;i<=n;i++) scanf("%d",&w[i]);
  build(1,1,n);
  
  int k,x,y;
  while(m--){
    scanf("%d%d%d",&k,&x,&y);
    if(k==1){
      if(x>y) swap(x,y);
      printf("%d\n",query(1,x,y).tmax);
    }else modify(1,x,y);
  }
  return 0;
}
```



#### 3.3 AcWing 246. 区间最大公约数   (todo)

**[题目：246. 区间最大公约数 ]()**

**题目描述**

给定一个长度为 *N* 的数列 *A*，以及 *M* 条指令，每条指令可能是以下两种之一：

1、`C l r d`，表示把 $A[l]$,$A[l+1]$,…,$A[r]$都加上 $d$。

2、`Q l r`，表示询问 $A[l]$,$A[l+1]$,…,$A[r]$的最大公约数(GCD）

对于每个询问，输出一个整数表示答案。

**输入格式**

第一行两个整数 *N*,*M*。

第二行 *N*个整数 *A*[*i*]。

接下来 *M*行表示 *M* 条指令，每条指令的格式如题目描述所示。

**输出格式**

对于每个询问，输出一个整数表示答案。

每个答案占一行。

**数据范围**

$N≤500000,M≤100000$,
$1≤A[i]≤10^{18}$,
 $|d|≤10^{18}$

**输入样例：**

```r
5 5
1 3 5 7 9
Q 1 5
C 1 5 1
Q 1 5
C 3 3 6
Q 2 4
```

**输出样例：**

```
1
2
4
```

**题解：**

``` c
怎么将求解一个区间的操作转化为只对某一个点
最大公约数左边等于右边：
(x,y,z)= (x,y-x,z-y)    
(a1, a2, .....an) <= (a, a2-a1, a3-a2, ....an-an-1)
(a1, a2, .....an) >= (a, a2-a1, a3-a2, ....an-an-1)
(a1, a2, .....an) = (a, a2-a1, a3-a2, ....an-an-1) = d  
    
求[l,r]的最大公约数：
gcd(a[l], gcd{b[l+1]~b[r]})
    
用差分来存sum和gcd
```

**代码：**

```c
#include <cstdio>
#include <cstring>
#include <iostream>
#include <algorithm>

using namespace std;

typedef long long LL;

const int N = 500010;

int n, m;
LL w[N];
struct Node
{
    int l, r;
    LL sum, d;
}tr[N * 4];

LL gcd(LL a, LL b)
{
    return b ? gcd(b, a % b) : a;
}

void pushup(Node &u, Node &l, Node &r)
{
    u.sum = l.sum + r.sum;
    u.d = gcd(l.d, r.d);
}

void pushup(int u)
{
    pushup(tr[u], tr[u << 1], tr[u << 1 | 1]);
}

void build(int u, int l, int r)
{
    if (l == r)
    {
        LL b = w[r] - w[r - 1];
        tr[u] = {l, r, b, b};
    }
    else
    {
        tr[u].l = l, tr[u].r = r;
        int mid = l + r >> 1;
        build(u << 1, l, mid), build(u << 1 | 1, mid + 1, r);
        pushup(u);
    }
}

void modify(int u, int x, LL v)
{
    if (tr[u].l == x && tr[u].r == x)
    {
        LL b = tr[u].sum + v;
        tr[u] = {x, x, b, b};
    }
    else
    {
        int mid = tr[u].l + tr[u].r >> 1;
        if (x <= mid) modify(u << 1, x, v);
        else modify(u << 1 | 1, x, v);
        pushup(u);
    }
}

Node query(int u, int l, int r)
{
    if (tr[u].l >= l && tr[u].r <= r) return tr[u];
    else
    {
        int mid = tr[u].l + tr[u].r >> 1;
        if (r <= mid) return query(u << 1, l, r);
        else if (l > mid) return query(u << 1 | 1, l, r);
        else
        {
            auto left = query(u << 1, l, r);
            auto right = query(u << 1 | 1, l, r);
            Node res;
            pushup(res, left, right);
            return res;
        }
    }
}

int main()
{
    scanf("%d%d", &n, &m);
    for (int i = 1; i <= n; i ++ ) scanf("%lld", &w[i]);
    build(1, 1, n);

    int l, r;
    LL d;
    char op[2];
    while (m -- )
    {
        scanf("%s%d%d", op, &l, &r);
        if (*op == 'Q')
        {
            auto left = query(1, 1, l);
            Node right({0, 0, 0, 0});
            if (l + 1 <= r) right = query(1, l + 1, r);
            printf("%lld\n", abs(gcd(left.sum, right.d)));
        }
        else
        {
            scanf("%lld", &d);
            modify(1, l, d);
            if (r + 1 <= n) modify(1, r + 1, -d);
        }
    }

    return 0;
}
```



#### (2) 赖标记、pushdown

单点修改一般pushup操作就够了

区间修改则需要懒标记-pushdown，思想来源于区间查询

```c
1、修改操作
信息：
    （1）sum:当前区间的总和
    （2）add懒标记，给以当前节点为根的子树中的每个节点，加上add(不包含根节点)
        当前的操作到该根节点之后就不在传播了，直接返回
        
2、查询操作
     查询某一段区间的时候，如果用到的某一个值被add懒标记过了，则需要找到该子节点的父节点，一开始加add懒标记的节点，再将祖宗的root所有的add操作累加到所求点位置，那么可以找到祖宗root节点，每次递归左右儿子加add懒标记，每次懒标记之后清空当前的add懒标记，一直pushdown向下。
     假设祖宗节点root被add懒标记了，root.add
     pushdown操作：
     left.add += root.add;
	 left.sum +=(left.R-left.R+1)*root.add	//	左儿子总和=左边节点个数*add
     right.add += root.add;
     right.sum +=(right.R-right.R+1)*root.add	//	左儿子总和=左边节点个数*add
     root.add = 0  //清空懒标记
```



#### 3.4  Acwing243. 一个简单的整数问题2             

**[题目：243. 一个简单的整数问题2 ]()**

**题目描述**

给定一个长度为 $N$ 的数列 $A$，以及 $M$条指令，每条指令可能是以下两种之一：

1. `C l r d`，表示把 $A[l]$,$A[l+1]$,…,$A[r]$都加上 $d$。

​    2.`Q l r`，表示询问数列中第 `l∼r`个数的和。

对于每个询问，输出一个整数表示答案。

**输入格式**

第一行两个整数 *N*,*M*。

第二行 *N*个整数 *A*[*i*]。

接下来 *M*行表示 *M* 条指令，每条指令的格式如题目描述所示。

**输出格式**

对于每个询问，输出一个整数表示答案。

每个答案占一行。

**数据范围**

$1≤N,M≤10^5$,
 $|d|≤10000$,
 $|A[i]|≤10^9$,

**输入样例：**

```r
10 5
1 2 3 4 5 6 7 8 9 10
Q 4 4
Q 1 10
Q 2 4
C 3 6 3
Q 2 4
```

**输出样例：**

```
4
55
9
15
```

**题解：**

``` c
维护:
    sum : 如果只考虑当前节点及子节点上的所有标记，当前区间和是多少（没有考虑所有祖先节点上的标记，只考虑当前点及儿子）
    add : 给当前区间的所有儿子加上add
```

**代码：**

```c
#include <cstring>
#include <cstdio>
#include <algorithm>
#include <iostream>
typedef long long LL;
using namespace std;
const int N = 1e5+5;
int w[N];
int n,m;

struct Node{
  int l,r;  //左右区间
  LL sum,add;  //区间里面的总和，以及懒标记
}tr[N*4];

void pushup(int u){
  tr[u].sum = tr[u<<1].sum + tr[u<<1|1].sum;
}

void pushdown(int u){
  auto& root=tr[u],&left=tr[u<<1],&right=tr[u<<1|1];
  //如果存在懒标记
  if(root.add){
    left.add+=root.add, left.sum+=(LL)(left.r-left.l+1)*root.add;
    right.add+=root.add,right.sum+=(LL)(right.r-right.l+1)*root.add;
    //将懒标记清除
    root.add=0;
  }
}

void build(int u,int l,int r){
  if(l==r){
    //懒标记刚build的时候0
    tr[u]={l,r,w[r],0};
  }else{
    tr[u]={l,r}; //不加会段错误
    int mid = l+r>>1;
    build(u<<1,l,mid),build(u<<1|1,mid+1,r);
    pushup(u);
  }
}

void modify(int u,int l,int r,int d)
{
  //包含区间
  if(tr[u].l>=l && tr[u].r<=r){
    tr[u].sum+=(LL)(tr[u].r-tr[u].l+1)*d;
    tr[u].add+=d;
  }else{  //需要分裂，可能存在两边的add数值不一致
    pushdown(u);
    int mid=tr[u].l+tr[u].r>>1;
    if(l<=mid) modify(u<<1,l,r,d);
    if(r>mid) modify(u<<1|1,l,r,d);
    pushup(u);
  }
}

LL query(int u,int l,int r){
  if(tr[u].l>=l && tr[u].r<=r) return tr[u].sum;
  
  pushdown(u);
  int mid=tr[u].l+tr[u].r>>1;
  LL sum=0;
  if(l<=mid) sum=query(u<<1,l,r);
  if(r>mid) sum+=query(u<<1|1,l,r);
  return sum;
}

int main()
{
  scanf("%d%d",&n,&m);
  for(int i=1;i<=n;i++) scanf("%d",&w[i]);
  build(1,1,n);
  
  char op[2];
  int l,r,d;
  
  while(m--){
    scanf("%s%d%d",op,&l,&r);
    if(*op=='C'){
      scanf("%d",&d);
      modify(1,l,r,d);
    }else printf("%lld\n",query(1,l,r));
  }
  
  return 0;
}
```





#### 3.5 AcWing 247. 亚特兰蒂斯            

**[题目：AcWing 247. 亚特兰蒂斯 ]()**

**题目描述**

有几个古希腊书籍中包含了对传说中的亚特兰蒂斯岛的描述。 

其中一些甚至包括岛屿部分地图。 

但不幸的是，这些地图描述了亚特兰蒂斯的不同区域。 

您的朋友 Bill 必须知道地图的总面积。 

你自告奋勇写了一个计算这个总面积的程序。

**输入格式**

输入包含多组测试用例。

对于每组测试用例，第一行包含整数 $n$，表示总的地图数量。

接下来 $n$ 行，描绘了每张地图，每行包含四个数字 $x1,y1,x2,y2$（不一定是整数），$(x1,y1)$ 和 $(x2,y2)$分别是地图的左上角位置和右下角位置。

注意，坐标轴 $x$轴从上向下延伸，$y$轴从左向右延伸。

当输入用例 $n$=0时，表示输入终止，该用例无需处理。

**输出格式**

每组测试用例输出两行。

第一行输出 `Test case #k`，其中 $k$是测试用例的编号，从 1开始。

第二行输出 `Total explored area: a`，其中 *a*是总地图面积（即此测试用例中所有矩形的面积并，注意如果一片区域被多个地图包含，则在计算总面积时只计算一次），精确到小数点后两位数。

在每个测试用例后输出一个空行。

**数据范围**

$1≤n≤10000$,
 $0≤x1<x2≤100000$,
 $0≤y1<y2≤100000$
 注意，本题 $n$ 的范围上限加强至 10000。

**输入样例：**

```r
2
10 10 20 20
15 15 25 25.5
0
```

**输出样例：**

```
Test case #1
Total explored area: 180.00 
```

#### 样例解释

样例所示地图覆盖区域如下图所示，两个矩形区域所覆盖的总面积，即为样例的解。

![](https://cdn.acwing.com/media/article/image/2019/12/26/19_4acba44c27-%E6%97%A0%E6%A0%87%E9%A2%98.png)**题解：**

``` c

```

**代码：**

```c

```



#### 3.6 AcWing 245. 你能回答这些问题吗      

**[题目：AcWing 245. 你能回答这些问题吗  ]()**

**题目描述**

给定 $n $个正整数 $ai$，判定每个数是否是质数。

**输入格式**

第一行包含整数 $n$。

接下来 $n$ 行，每行包含一个正整数 $a_i$。

**输出格式**

共 n 行，其中第 i 行输出第 i 个正整数 ai 是否为质数，是则输出 `Yes`，否则输出 `No`。

**数据范围**

$1≤n≤100$,
$1≤ai≤2^{31}−1$

**输入样例：**

```r
2
2
6
```

**输出样例：**

```
Yes
No
```

**题解：**

``` c

```

**代码：**

```c

```



#### 

### （3）扫描线

