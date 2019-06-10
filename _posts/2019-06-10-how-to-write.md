---
layout: post
title: 拓扑排序
date: 2019-6-10
categories: blog
tags: [算法,uva]
description: 语言
---
# 拓扑排序
在一个有向图中，对所有的节点进行排序，要求没有一个节点指向它前面的节点。

先统计所有节点的入度，对于入度为0的节点就可以分离出来，然后把这个节点指向的节点的入度减一。

一直做改操作，直到所有的节点都被分离出来。

如果最后不存在入度为0的节点，那就说明有环，不存在拓扑排序，也就是很多题目的无解的情况。

下面是算法的演示过程。

！[拓扑排序](img/拓扑排序.png)

拓扑排序是一个比较常用的图论算法，经常用于完成有依赖关系的任务的排序。
举个栗 例子：有人想要制作一件工具，但是这个工具不是一次就可以完成的，分很多个步骤，而且这些步骤是有顺序的，也就是说，假设B的顺序在A的后面，那么你就必须要先完成A再完成B，但是也有些步骤不分顺序，意思是你先做哪一个都是可以的。
面对这样的问题，我们可以把步骤建立成一张有向无环图，A指向B意思是A要在B前面完成，那么下面，我们就要找到一个顺序，来使答案符合题目要求。拓扑排序就是干这样的事情的。

拓扑排序实现思路：先从第一个没有入度的节点开始（如果有多个则任意），将此点放入队列中，并且删除与之有关的边，再重复上述步骤，直到所有的点全部进入队列，此时输出即可。

### 算法实现：

（1）若图中的点入度均大于0则不存在拓扑序列，否则进行第二步

（2）取一个入度为0的点u并将其放置序列末尾

（3）删除点u以及从u伸出的边，即将与u相连的点的入度减1

（4）若图中还存在顶点，再从（1）开始

代码实现（模板）：

    #include<cstdio> 
    #include<vector>
    #include<cstring>
    #include<queue>
    #include<iostream>
    using namespace std;
    const int maxn=1e3;
    vector<int>grid[maxn];
    int indu[maxn];
    void find(int n){
	queue<int>Q;
	for(int i=1;i<=n;i++)if(indu[i]==0)Q.push(i),cout<<i<<",";
	while(!Q.empty()){
		int now=Q.front();Q.pop();
		for(int i=0;i<grid[now].size();i++){
			int next=grid[now][i];
			if(--indu[next]==0)Q.push(next),cout<<next<<",";
		}
	}
	cout<<endl;
    }
    int main(){
	int n,m;
	while(~scanf("%d%d",&n,&m)){
		for(int i=1;i<=n;i++)grid[i].clear();
		memset(indu,0,sizeof(indu));
		for(int i=0;i<m;i++){
			int p1,p2;
			scanf("%d%d",&p1,&p2);
			grid[p1].push_back(p2);//p1到p2有一条边
			indu[p2]++;
		}
		find(n);
	}
	return 0;
    }

### 实战：紫书uva10305给任务排序

#### 题意
假设有n个变量，还有m个二元组(u,v)，分别表示u小于v。那么，所有变量从小到大排列是什么样子呢？例如，有四个变量a,b,c,d。若已知a < b,c < b,d < c,则这四个变量排序可能是a< d < c < d。尽管还有其他可能(如d < a < c < b)，你只需要算其中一个即可。

#### 输入
分别输入m,n,m表示1到m个数，n表示有n对关系。0，0结束</br>
5 4</br>
1 2</br>
2 3</br>
1 3</br>
1 5</br>
0 0</br>

#### 输出

1 4 2 5 3

#### 分析
把每个变量看成一个点，小于关系看为边，则得到了一个有向图，我们的任务就变成了个图的所有结点排序。

   紫书上用的dfs遍历结点，我用队列保存入度为0的结点，显得更好理解，只不过相对书上代码比较复杂。


代码加注释：</br>

    #include<bits/stdc++.h>
    using namespace std;
    vector<int> map1[101];
    int indu[101];
    int m,n;
    void solve()
    {
    queue<int> q;
     vector<int> ans;
    for(int i=1;i<=m;i++)
    {
        if(indu[i]==0)//入读为零表示为根结点
            {
                q.push(i);//加入队列
            }
    }
    while(!q.empty())//类似BFS（紫书是DFS）
    {
       int y=q.front();
       q.pop();
       ans.push_back(y);
       for(int i=0;i<map1[y].size();i++)//遍历根节点所连的点
       {
           int x=map1[y][i];
           indu[x]--;//入读减一
           if(indu[x]==0)
           {
               q.push(x);
               //ans.push_back(x);
           }
       }
    }
    for(int i=0;i<ans.size();i++)
    {
       cout<<ans[i]<<" ";
    }
    for(int i=1;i<=m;i++)
    {
       map1[i].clear();
    }
    cout<<endl;
    }
    int main()
    {
    int i,j;
    while(cin>>m>>n)
    {
        if(m==0&&n==0)
            break;
        memset(indu,0,sizeof(indu));
    for(i=1;i<=n;i++)
    {
        int a,b;
        cin>>a>>b;
        map1[a].push_back(b);//表示a的所连接的点
        indu[b]++;//b点的入度
    }
    solve();
    }
    }


** 嘿嘿，刷题60了啊，继续加油，冲冲冲！！！！！







