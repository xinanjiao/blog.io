---
layout: post
title: TSP(双调旅行商问题) 动态规划
date: 2020-02-08
categories: blog
tags: [动态规划]
description: 语言
---

### 关于TSP旅行商问题
这个问题源于紫书uva 1347问题。<br>
看书上是一种动态规划问题，又在DAG上的动态规划那一节，不自主的向DAG固定终点问题去想，但状态转移对我这个新手来说属实困难。寻遍网上对这道题的讲解，最后发现这是一道TSP双调旅行商的模板题，很经典，所以看了很多方面的讲解，谈谈我的理解。原址链接:<https://blog.csdn.net/qq742762377/article/details/85050040>

### 双调TSP问题通俗讲解
**欧几里得旅行商问题是对平面上给定的n个点确定一条连接各点的最短闭合旅程的问题，下图a给出了7个点问题的解。这个问题的一般形式是NP完全的，故其解需要多于多项式的时间。**
![tsp1](/img/tsp1.png) ![tsp2](/img/tsp2.png)


J.L.Bentley建议通过只考虑双调旅程来简化问题**这种旅程即为从最左点开始，严格地从左到右直至最右点，然后严格地从右到左直至出发点**。b显示了同样7个点问题的最短双调路线。在这种情况下，多项式时间的算法是可能的。

描述一个确定最优双调路线的O(n^2)时间的算法，可以假设任何两点的x坐标都不相同。


将各个节点从左到右排序，编号为1，2，3，.....，n。对于任意的i和j（其中1<=i,j<=n）。

现在有两条路径A、B都是从点1出发，A走的路径为1~ i ，B走的路径为1~ j ，

即A、B有公共的起点，**但途中没有交叉点(即终点之前不存在 i=j )**，终点可能重合(i=j)，也可能不重合(i≠j)，这取决与我们要求的问题。

令s=max(i,j)，则从1到s所有的点一定在路径A或者路径B上，不会有遗漏的点.

对于特定的 i 和 j ，路径A、B存在多种可能的走法，其中比有一种2条路径的和最小的走法，我们把这2条路径的和记为b[i,j]；当i=j时，b[i,j]表示了从1到 i 的双调TSP的解；当i=j=n时 b[i,j] 就表示了整个问题的最终解法。

我们可以采用DP(动态规划)求解

1. i > j 
  ![tsp3](/img/tsp3.png)
  已知b[i,j] ,求b[i+1,j]，只要将A直接延长到 i+1就行。
  即 **b[i+1,j] = b[i,j] + distance(i,i+1)**

2. i = j 
  假设已知b[i，i]，求b[i+1，i],可以想象，此时AB两条路径在终点 i 相交，因为现在我们要求A的终点为i+1，所以不得不把相交的AB在i点拆开。 
  ![tsp4](/img/tsp4.png)
  ![tsp5](/img/tsp5.png)
  事情没那么简单，拆开后，我们需要在路径A找任意点u(0<=u<=i)，使点u连接点i+1.
  ![tsp6](/img/tsp6.png)
  我们需要找到 **min{ b[u,j] + distance（u, i+1）},即遍历 b[1..i, j]**，刚好这是图的左下部分，因为这个部分是和右上部分对称的，所以可以直接利用右上部分的数据。
  **b[i+1,j] = min{ B[u,i] + w(u,i+1) }**

3. 确定b[n,n]，即确定最终的解。
  前面 ①②部分已经可以把所有的点都走一次了，现在我们最后需要做的是，**把AB路径的头连接起来**。
  ![tsp7](/img/tsp7.png)

那怎么连接才能让AB路径加起来最短呢？

前面我们说过: s=max(i,j)，则从1到s所有的点一定在路径A或者路径B上，不会有遗漏的点

通过①②我们已经求出 1 ~ s 点的所有数据。

也就是我们可以通过 **min{ b[n，k] + distance（n，k）} 其中 1<=k< n 求出b[n,n]**；

**b[n,n] =  min{ b[n，k] + distance（n，k）}**

总结：<br>
b[i,j] = b[i-1,j] + distance(i-1,i) 

b[i,j] = min{ B[u,i] + w(u,i+1) } 

b[n,n] =  min{ b[n，k] + distance（n，k）}  0<=k< n



### 例题
uva 1347 poj 2677 hdu 2224都是TSP模板的板子题，在这里总结板子如下：

'''

	struct TSP{
	int x[maxn],y[maxn],n;
	double dis[maxn][maxn];
	double dp[maxn][maxn];
	void Distance(int x1,int x2,int y1,int y2){return sqrt((x1-x2)*(x1-x2)+(y1-y2)*(y1-y2));}
    void DP(){
        dp[1][2]=dis[1][2];
        for(int j=3;j<=n;j++){
            //i!=j-1
            for(int i=1;i<j-1;i++)
                dp[i][j]=dp[i][j-1]+dis[j-1][j];
             //i==j-1
            double mina=INF;
            for(int k=1;k<j-1;k++)
                mina=min(mina,dp[k][j-1]+dis[k][j]);
            dp[j-1][j]=mina;
        }
       //最后汇总
        dp[n][n]=dp[n-1][n]+dis[n-1][n];
    }
    int init(){
        if(scanf("%d",&n)==-1)
            return -1;
        else{
            fro(i,1,n+1){
                cin>>x[i]>>y[i];
            }
            fro(i,1,n+1)
                fro(j,1,n+1)
                    dis[i][j]=Distance(x[i],x[j],y[i],y[j]);
            DP();
            printf("%.2lf\n",dp[n][n]);
        }
    }
    }s;

'''
和上面讲的异曲同工之妙！！！转换一下就好了