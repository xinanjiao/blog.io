---
layout: post
title: DAG上的动态规划
date: 2020-2-04
categories: blog
tags: [动态规划]
description: 语言
---

转自博客：<https://blog.csdn.net/qq_42815188/article/details/101934040><br>

## DAG上的最长最短路

待解决问题：

1. 求整个DAG中的最长路径 (即不固定起点跟终点)。
2. 固定终点，求DAG的最长路径。

## 一、求整个DAG中的最长路径（即不固定起点或终点）
先讨论第一个问题：给定一个有向无环图，怎样求解整个图的所有路径中权值之和最大的那条。如图11-6所示，B→D→F→I 就是该图的最长路径，长度为9。
![dp](/img/DAG1.png)
###问题一：如何定义数据结构？
令 dp[i]表示**从i号顶点出发能获得的最长路径长度** ，这样所有dp[i]的最大值就是整个DAG的最长路径长度。

### 问题二：如何求解dp数组？
意到 dp[i] 表示从 i 号顶点出发能获得的最长路径长度，如果从 i 号顶点出发能直接到达顶点 j1、j2、……jk，而 dp[j1]、dp[j2]……dp[jk] 均已知，那么就有 dp[i] = max{ dp[j] + length [i->j] | (i, j)∈E }，如图11-7所示。
![dp](/img/DAG2.png)
### 解法1——逆拓扑序列
显然，根据上面的思路，需要按照逆拓扑序列的顺序来求解dp数组。

### 解法2——递归
有没有办法不求出逆拓扑序列也能计算dp数组的方式呢？
当然，那就是递归。

代码如下，其中使用邻接矩阵的方式来存储图：

	int DP(int i){
	if(dp[i] > 0) return dp[i];	//dp[i]已计算得到
	for(int j=0;j<n;j++){	//遍历i的所有出边 
		if(G[i][j] != INF){
			dp[i] = max(dp[i],DP(j)+G[i][j]);
		}
	}
	return dp[i];	//返回计算完毕的dp[i] 
	}


由于从出度为0的顶点出发的最长路径长度为0，因此边界为这些顶点的dp值为0。但具体实现中不妨对整个 dp 数组初始化为0，这样 dp 函数当前访问的顶点 i 的出度为0时就会返回 dp[i] = 0 (以此作为dp的边界)，而出度不是0的顶点则会递归求解，递归过程中遇到已经计算过的顶点则直接返回对应的dp值，于是从程序逻辑上按照了逆拓扑序列的顺序进行。希望读者能认真体会上面代码的思想。

### 问题三：如何知道最长路径具体是哪条呢？

回忆在 Dijkstra 算法中是如何求解最短路径的。—— 开了一个int 型数组pre，来记录每个顶点的前驱，每当发现更短的路径时对 pre 进行修改。事实上，可以把这种想法应用于求解最长路径上 —— 开一个 int 型 choice 数组记录最长路径上顶点的后继顶点，这样就可以像 Dijkstra 算法中那样来求解最长路径了，只不过由于 choice 数组存放的是后继顶点，因此使用迭代即可 (当然使用递归也是可以的)，如下面的代码所示。如果最终可能有多条最长路径，将 choice 数组改为 vector 类型的数组即可 (也就是Dijkstra算法中有多条最短路径时的做法)，代码如下：

方法一：

	int DP(int i){
	if(dp[i] > 0 )  return dp[i];	//dp[i]已计算得到
	for(int j=0;j<n;j++){	//遍历i的所有出边 
		if(G[i][j] != INF){
			int temp = DP(j) + G[i][j];	//单独计算，防止if中调用
			if(temp > dp[i]){	//可以获得更长的路径 
				dp[i] = temp;	//覆盖dp[i]
				choice[i] = j;	//i号顶点的后继顶点是j 
			} 
		} 
	}
	return dp[i];	//返回计算完毕的dp[i] 
	}
	//调用printPath前需要先得到最大的dp[i],然后将i作为路径起点传入
	void printPath(int i){
	printf("%d",i);
	while(choice[i] != -1){	//choice数组初始化为-1 
		i = choice[i];
		printf("->%d",i); 
	}
	} 


方法二（紫书代码）：

    void print(int i){
    	print("%d ",i);
    	for(int j=0;i<n;j++){
    		if(mp[i][j]&&dp[j]=dp[i]+1){
    			print(j);
    			break;
    		}
    	}
    }

对一般的动态规划问题而言，如果需要得到具体的最优方案，可以采用类似的方法，即**记录每次决策所选择的策略，然后在 dp 数组计算完毕后根据具体情况进行递归或者迭代来获取方案。**

更进步，模仿字符串来定义**路径序列的字典序**：如果有两条路径 a1→a2→……→am 与 b1→b2→……→bn，且a1=b1、a2=b2、 … ak=bk、 ak+1 < bk+1， 那么称路径序列 a1→a2→……→am 的字典序小于路径 b1→b2→……→bn。于是以此可以提出这样一个问题：如果DAG中有多条最长路径，如何选取字典序最小的那条？很简单，只需要让遍历 i 的邻接点的顺序从小到大即可 (事实上，上面的代码自动实现了这个功能，想一想为什么? )。

至此，都是令 dp[j] 表示从 i 号顶点出发能获得的最长路径长度。那么，如果令 dp[i] 表示以 i 号顶点结尾能获得的最长路径长度，又会有什么结果呢？
可以想象，只要把求解公式变为 **dp[i] = max{dp[j] + lengh[j→i] | (j, i)∈E} (相应的求解顺序变为拓扑序)**，就可以同样得到最长路径长度，也可以设置 choice 数组求出具体方案，但却不能直接得到字典序最小的方案，这是为什么呢？

![DAG](/img/DAG3.png)

举个很简单的例子，如图11-8所示，如果令 dp[i] 表示从 i 号顶点出发能获得的最长路径长度，且 dp[2] 和 dp[3] 已经计算得到，那么计算 dp[1] 的时候只需要从 V2 和 V3 中选择字典序较小的 V2 即可；而如果令 dp[i] 表示以 i 号顶点结尾能获得的最长路径长度，且 dp[4] 和 dp[5] 已经计算得到，那么计算 dp[6] 时如果选择了字典序较小的 V4，则会导致错误的选择结果：理论上应当是 V1→V2→V5 的字典序最小，可是却选择了 V1→V3→V4。显然，**由于字典序的大小总是先根据序列中较前的部分来判断，因此序列中越靠前的顶点，其 dp 值应当越后计算**(对一般的序列型动态规划问题也是如此)。

## 二、固定终点，求DAG的最长路径长度
在上面的讨论的基础上，接下来讨论本节开头的第二个问题：**固定终点，求DAG的最长路径长度**。例如在图11-6中，如果固定H为路径的终点，那么最长路径就会变成B→D→F→H。

有了上面的经验，应当能很容易想到这个延伸问题的解决方案。假设规定的终点为T，那么可以令dp[i]表示**从i号顶点出发到达终点T能获得的最长路径长度**。

![DAG](/img/DAG4.png)

	int DP(int i){
	if(vis[i]) return dp[i];	//dp[i]已计算得到
	vis[i] = true;
	for(int j=0;j<n;j++){
		if(G[i][j] != INF){
			dp[i] = max(dp[i],DP(j) + G[i][j]);
		}
	}
	return dp[i];	//返回计算完毕的dp[i] 
	}


至于如何记录方案以及如何选择字典序最小的方案，均与第一个问题相同，此处不再赘述。读者需要思考，如果令dp[i]表示以i号顶点结尾能获得的最长路径长度，应当如何处理？
事实上这样设置dp[i]会变得更容易解决问题，并且dp[T]就是结果，只不过仍然不方便处理字典序最小的情况。

至此，DAG最长路的两个关键问题都已经解决，最短路的做法与之完全相同。那么具体什么场景可以应用到呢？


### 典例1 矩形嵌套问题 计蒜客 - T1408  
### 题目大意
给出n个矩阵的长和宽，定义矩阵的嵌套关系为：如果有两个矩形A和B，其中矩形A的长和宽分别为a、b，矩形B的长和宽分别为从从c、d，且满足a< c、b< d,或a< d、b< c，则称矩形A可以嵌套于矩形B内。现在要求一个矩形序列，使得这个序列中任意两个相邻的矩形都满足前面的矩形可以嵌套于后一个矩形内，且序列的长度最长。如果有多个这样的最长序列，选择矩形编号序列的字典序最小的那个。

### 思路
没叫输出路径，只输出最大的个数，参考上面全部DAG路径的最大长度。先建图。

	struct node{
    int x,y;
    node(int a,int b):x(a),y(b){}
    node(){}
	}a[1100];
	bool mp[1100][1100];
	int dp[1100];
	int n;
	bool cheack(node a,node b){
    if((a.x<b.x&&a.y<b.y)||(a.x<b.y&&a.y<b.x))
        return true;
    else
        return false;
	}
	int Dp(int i){
    if(dp[i]>0)return dp[i];
    fro(j,0,n){
        if(mp[i][j]){
            dp[i]=max(dp[i],Dp(j)+1);
        }
    }
    return dp[i];
	}
	int main(){
    ios::sync_with_stdio(0);
     int t;
     cin>>t;
     while(t--){
        cin>>n;
        mem(mp,0);
        mem(dp,0);
        fro(i,0,n){
            cin>>a[i].x>>a[i].y;
        }
        fro(i,0,n){
            fro(j,0,n){
                if(i!=j)
                    mp[i][j]=cheack(a[i],a[j]);
            }
        }
        fro(i,0,n)
          Dp(i);
          int maxa=-1;
        fro(i,0,n)
        maxa=max(maxa,dp[i]);
       cout<<maxa+1<<endl;
     }
    return 0;
	}


### 典例二 ：硬币问题 规定终点DAG问题 Coin Changing Problem Aizu - DPL_1_A
### 题目大意
现有面值为c_1,c_2,...,c_m元的m种硬币，求支付n元时所需硬币的最少枚数。各面值的硬币可重复使用任意次。

### 思路
参考DAG模型规定终点的情况。有两种解法：递归和递推


	int n,m;
	int dp[50020],a[30];
	bool vis[50020];
	int Dp(int aa){
    if(vis[aa])return dp[aa];
    vis[aa]=1;
    fro(j,0,m)
      if(aa>=a[j])
        dp[aa]=min(dp[aa],Dp(aa-a[j])+1);
    return dp[aa];
	}
	int main(){
    ios::sync_with_stdio(0);
     cin>>n>>m;
     fro(i,0,m)
      cin>>a[i];
      mem(dp,INF);
      mem(vis,0);
      dp[0]=0;
      fro(i,1,n+1){
        fro(j,0,m+1){
            if(i>=a[j])
                dp[i]=min(dp[i],dp[i-a[j]]+1);
        }
      }
      cout<<dp[n]<<endl;
    return 0;
	}