---
layout: post
title: 线段树
date: 2019-8-10
categories: blog
tags: [算法,训练,数据结构]
description: 语言
---
参考博客：<https://www.cnblogs.com/TheRoadToTheGold/p/6254255.html><br/>
         <https://blog.csdn.net/zearot/article/details/48299459><br/>

## 引例
<p style="color: rgb(176,196,222);">A.给出n个数，n<=100，和m个询问，每次询问区间[l，r]的和，并输出。</p>

一种回答：这也太简单了，O（n）枚举搜索就行了。

另一种回答：还用得着o(n）枚举，前缀和o(1)就搞定。

那好，我再修改一下题目。

<p style="color: rgb(189,183,107);">B.给出n个数，n<=100，和m个操作，每个操作可能有两种：1、在某个位置加上一个数；2、询问区间[l，r]的和，并输出。

回答：o（n）枚举。</p>

动态修改最起码不能用静态的前缀和做了。

好，我再修改题目：

<p style="color: rgb(255,192,203);">C.给出n个数，n<=1000000，和m个操作，每个操作可能有两种：1、在某个位置加上一个数；2、询问区间[l，r]的和，并输出。</p>

回答：o（n）枚举绝对超时。

再改：

<p style="color: green;">D，给出n个数，n<=1000000，和m个操作，每个操作修改一段连续区间[a,b]的值</p>

回答：从a枚举到b，一个一个改。。。。。。有点儿常识的人都知道超时

那怎么办？这就需要一种强大的数据结构：<p style="color: red;">线段树。</p>

## 原理

线段树的原理，就是，将[1,n]分解成若干特定的子区间(数量不超过4 * n),然后，将每个区间[L,R]都分解为
少量特定的子区间，通过对这些少量子区间的修改或者统计，来实现快速对[L,R]的修改或者统计。<br/>

由此看出，用线段树统计的东西，必须符合区间加法，否则，不可能通过分成的子区间来得到[L,R]的统计结果。<br/>

**符合区间加法的例子：**<br/>
数字之和——总数字之和 = 左区间数字之和 + 右区间数字之和<br/>
最大公因数(GCD)——总GCD = gcd( 左区间GCD , 右区间GCD );<br/>
最大值——总最大值=max(左区间最大值，右区间最大值)<br/>
**不符合区间加法的例子：**<br/>
众数——只知道左右区间的众数，没法求总区间的众数<br/>
01序列的最长连续零——只知道左右区间的最长连续零，没法知道总的最长连续零<br/>

线段树本质上是维护下标为1,2,..,n的n个按顺序排列的数的信息，所以，其实是“点树”，是维护n的点的信息，至于每个点的数据的含义可以有很多，
在对线段操作的线段树中，每个点代表一条线段，在用线段树维护数列信息的时候，每个点代表一个数，但本质上都是每个点代表一个数。以下，在讨论线段树的时候，区间[L,R]指的是下标从L到R的这(R-L+1)个数，而不是指一条连续的线段。只是有时候这些数代表实际上一条线段的统计结果而已。


线段树是将每个区间[L,R]分解成[L,M]和[M+1,R](其中M=(L+R)/2 这里的除法是整数除法，即对结果下取整)直到 L==R 为止。

下图是[1,13]的分解过程
![tree](/img/tree.png)
由上图可得，

1、每个节点的左孩子区间范围为[l，mid]，右孩子为[mid+1,r]

2、对于结点k，左孩子结点为2*k，右孩子为2*k+1，这符合完全二叉树的性质

## 线段树的基本操作
建树，区间修改，区间查询，单点查询，单点修改

### 建树（递归版）
1 定义

    #define maxn 100007  //元素总个数
    #define ls l,m,rt<<1
    #define rs m+1,r,rt<<1|1
    int Sum[maxn<<2],Add[maxn<<2];//Sum求和，Add为懒惰标记 
    int A[maxn],n;//存原数组数据下标[1,n] 

2 建树

    //PushUp函数更新节点信息 ，这里是求和
    void PushUp(int rt){Sum[rt]=Sum[rt<<1]+Sum[rt<<1|1];}
    //Build函数建树 
    void Build(int l,int r,int rt){ //l,r表示当前节点区间，rt表示当前节点编号
	if(l==r) {//若到达叶节点 
		Sum[rt]=A[l];//储存数组值 
		return;
	}
	int m=(l+r)>>1;
	//左右递归 
	Build(l,m,rt<<1);//等于rt*2
	Build(m+1,r,rt<<1|1);//等于rt*2+1
	//更新信息 
	PushUp(rt);
    }

### 单点修改
假设第L位置上的数加上C，即A[L]+=C；

    void Update(int L,int C,int l,int r,int rt){//l,r表示当前节点区间，rt表示当前节点编号
	if(l==r){//到叶节点，修改 
		Sum[rt]+=C;
		return;
	}
	int m=(l+r)>>1;
	//根据条件判断往左子树调用还是往右 
	if(L <= m) Update(L,C,l,m,rt<<1);
	else       Update(L,C,m+1,r,rt<<1|1);
	PushUp(rt);//子节点更新了，所以本节点也需要更新信息 
    } 

### 区间修改
假设对区间L到R的所有数字加上C，即a[L,R]+=C;这里用到了懒惰标记，以后慢慢理解，先写下来

    void Update(int L,int R,int C,int l,int r,int rt){//L,R表示操作区间，l,r表示当前节点区间，rt表示当前节点编号 
	if(L <= l && r <= R){//如果本区间完全在操作区间[L,R]以内 
		Sum[rt]+=C*(r-l+1);//更新数字和，向上保持正确
		Add[rt]+=C;//增加Add标记，表示本区间的Sum正确，子区间的Sum仍需要根据Add的值来调整
		return ; 
	}
	int m=(l+r)>>1;
	PushDown(rt,m-l+1,r-m);//下推标记
	//这里判断左右子树跟[L,R]有无交集，有交集才递归 
	if(L <= m) Update(L,R,C,l,m,rt<<1);
	if(R >  m) Update(L,R,C,m+1,r,rt<<1|1); 
	PushUp(rt);//更新本节点信息 
    } 

懒惰标记,下推标记函数

    void PushDown(int rt,int ln,int rn){
	//ln,rn为左子树，右子树的数字数量。 
	if(Add[rt]){
		//下推标记 
		Add[rt<<1]+=Add[rt];
		Add[rt<<1|1]+=Add[rt];
		//修改子节点的Sum使之与对应的Add相对应 
		Sum[rt<<1]+=Add[rt]*ln;
		Sum[rt<<1|1]+=Add[rt]*rn;
		//清除本节点标记 
		Add[rt]=0;
	}
    }

### 区间查询
假设查询A[L,R]之和

    int Query(int L,int R,int l,int r,int rt){//L,R表示操作区间，l,r表示当前节点区间，rt表示当前节点编号
	if(L <= l && r <= R){
		//在区间内，直接返回 
		return Sum[rt];
	}
	int m=(l+r)>>1;
	//下推标记，否则Sum可能不正确
	PushDown(rt,m-l+1,r-m); 
	//累计答案
	int ANS=0;
	if(L <= m) ANS+=Query(L,R,l,m,rt<<1);
	if(R >  m) ANS+=Query(L,R,m+1,r,rt<<1|1);
	return ANS;
    } 


## 题目实战

### 敌兵布阵 
题目链接：<https://vjudge.net/contest/318111#problem/A><br/>
题目大意：<br/>
C国的死对头A国这段时间正在进行军事演习，所以C国间谍头子Derek和他手下Tidy又开始忙乎了。A国在海岸线沿直线布置了N个工兵营地,Derek和Tidy的任务就是要监视这些工兵营地的活动情况。由于采取了某种先进的监测手段，所以每个工兵营地的人数C国都掌握的一清二楚,每个工兵营地的人数都有可能发生变动，可能增加或减少若干人手,但这些都逃不过C国的监视。 
中央情报局要研究敌人究竟演习什么战术,所以Tidy要随时向Derek汇报某一段连续的工兵营地一共有多少人,例如Derek问:“Tidy,马上汇报第3个营地到第10个营地共有多少人!”Tidy就要马上开始计算这一段的总人数并汇报。但敌兵营地的人数经常变动，而Derek每次询问的段都不一样，所以Tidy不得不每次都一个一个营地的去数，很快就精疲力尽了，Derek对Tidy的计算速度越来越不满:"你个死肥仔，算得这么慢，我炒你鱿鱼!”Tidy想：“你自己来算算看，这可真是一项累人的工作!我恨不得你炒我鱿鱼呢!”无奈之下，Tidy只好打电话向计算机专家Windbreaker求救,Windbreaker说：“死肥仔，叫你平时做多点acm题和看多点算法书，现在尝到苦果了吧!”Tidy说："我知错了。。。"但Windbreaker已经挂掉电话了。Tidy很苦恼，这么算他真的会崩溃的，聪明的读者，你能写个程序帮他完成这项工作吗？不过如果你的程序效率不够高的话，Tidy还是会受到Derek的责骂的. <br/>

input：<br/>
第一行一个整数T，表示有T组数据。 <br/>
每组数据第一行一个正整数N（N<=50000）,表示敌人有N个工兵营地，接下来有N个正整数,第i个正整数ai代表第i个工兵营地里开始时有ai个人（1<=ai<=50）。 <br/>
接下来每行有一条命令，命令有4种形式： <br/>
(1) Add i j,i和j为正整数,表示第i个营地增加j个人（j不超过30） <br/>
(2)Sub i j ,i和j为正整数,表示第i个营地减少j个人（j不超过30）; <br/>
(3)Query i j ,i和j为正整数,i<=j，表示询问第i到第j个营地的总人数; <br/>
(4)End 表示结束，这条命令在每组数据最后出现; <br/>
每组数据最多有40000条命令 <br/>

output：<br/>

对第i组数据,首先输出“Case i:”和回车, 
对于每个Query询问，输出一个整数并回车,表示询问的段中的总人数,这个数保持在int以内。<br/>

思路：第一发用了vector模拟，当时不知道什么是线段树，自信满满的用vector做，我觉得肯定对的，果不其然会TLE，数据结构博大精深，看来得好好学，慢慢学，这道题也算线段树模板题<br/>

code

    using namespace std;
    #define ll long long int
    #define fro(i,a,n) for(ll i=a;i<n;i++)
    #define pre(i,a,n) for(ll i=n-1;i>=a;i--)
    #define mem(a,b) memset(a,b,sizeof(a))
    typedef pair<int,int> P;
    const double PI = 3.1415926535897932;
    const double EPS=1e-6;
    const int maxn=5e4+10;
    const int INF=0x3f3f3f3f;
    int tree[maxn*4];//线段树
    //int add[4*maxn];//懒惰标记（木洞）
    int a[maxn];//初始值
    int m;
    void bulidtree(int l,int r,int rt)
    {
    if(l==r)
    {
        tree[rt]=a[l];
        return ;
    }
    int mid=(l+r)/2;
    bulidtree(l,mid,rt*2);
    bulidtree(mid+1,r,rt*2+1);
    tree[rt]=tree[rt*2]+tree[rt*2+1];
    }
    void pushdown(int rt,int ln,int rn)//懒惰标记
    {
    }
    void add(int pos,int aa,int l,int r,int rt)
    {
    if(l==r)
    {
        tree[rt]+=aa;
        return ;
    }
    int mid=(l+r)/2;
    if(pos<=mid)
        add(pos,aa,l,mid,rt*2);//左子树
    else
        add(pos,aa,mid+1,r,rt*2+1);//右子树
    tree[rt]=tree[rt*2]+tree[rt*2+1];
    }
    ll query(int al,int ar,int l,int r,int rt)
    {
    if(al<=l&&ar>=r)//在区间内
    {
        return tree[rt];
    }
    int mid=(r+l)/2;
    ll sum=0;
    if(al<=mid)
        sum+=query(al,ar,l,mid,rt*2);
    if(ar>mid)
        sum+=query(al,ar,mid+1,r,rt*2+1);
    return sum;
    }
    int main()
    {
    ios::sync_with_stdio(false);
    int n;
    scanf("%d",&n);
    int case1=1;
    while(n--)
    {
        cout<<"Case "<<case1++<<":"<<endl;
        cin>>m;
        fro(i,1,m+1)
        {
            scanf("%d",&a[i]);
        }
        bulidtree(1,m,1);
        char s[10];
        while(scanf("%s",s)==1)
        {
            if(s[0]=='E')
                break;
            int aa,bb;
           scanf("%d %d",&aa,&bb);
           //cin>>aa>>bb;
             if(s[0]=='A')
            {
                add(aa,bb,1,m,1);
            }
            else if(s[0]=='S')
            {
                add(aa,-bb,1,m,1);
            }
            else if(s[0]=='Q')
            {
                printf("%lld\n",query(aa,bb,1,m,1));
            }
        }
    }
    }





