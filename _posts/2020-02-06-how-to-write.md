---
layout: post
title: codeforce 541 (Div. 2)(继续补题)
date: 2020-2-06
categories: blog
tags: [并查集,拓扑排序,深度搜索]
description: 语言
---

### D - Gourmet choice 并查集+拓扑排序
#### 题目大意
美食家第一天吃了n盘菜，第二天吃了m盘菜，美食家将两天的菜的美味程度比较写成n * m的方阵.
a[i][j]表示第一天第i盘菜和第二天第j盆菜的比较。请你给每盆菜打分，使每盆菜的分数尽量小，，且最小为1；
如果表格中关系矛盾输出“No”，否则输出“Yes”，并在第一第二行分别输出第一第二天个盆菜的评分；


#### 思路
如果没有等于那就是很简单的一道拓扑排序题。有了等于，意味着可以将一些状态放在一个集合用一个数代替。<br>
我们选择用并查集来做缩点操作。<br>
<p style=" color: red">记住;先将等于的点处理完之后在建图，拓扑排序</p>

关于输出NO的问题。
<p style="color: red">拓扑排序当中有环即表示不能有正常的比较，有歧义。判断方法：拓扑排序只会进行N次，不会小于那么多次，如果小于，即还有大于0度的点存在，即有环</p>

	int root[maxn<<1];
	char ch[maxn][maxn];
	vector<int> edge[maxn<<1];
	int answer[maxn<<1];
	int du[maxn<<1];
	int n,m,sum;
	int findroot(int i){
    return i==root[i]?root[i]:root[i]=findroot(root[i]);
	}
	void megeroot(int i,int j){
    int x=findroot(i);
    int y=findroot(j+n);
    if(x!=y){
        root[x]=y;
        sum++;
    }
	}
	void toolsort(){
    queue<int> q;
    fro(i,1,n+m+1){
        if(du[i]==0&&root[i]==i){
            sum++;
            answer[i]=1;//初始化为1
            q.push(i);
        }
    }
    while(!q.empty()){
        int a=q.front();
        q.pop();
        for(int i=0;i<edge[a].size();i++){
            du[edge[a][i]]--;
            //cout<<du[edge[a][i]]<<" "<<edge[a][i]<<endl;
            if(du[edge[a][i]]==0){
                    sum++;
                   // cout<<sum<<endl;
                answer[edge[a][i]]=answer[a]+1;
                q.push(edge[a][i]);
            }
        }
    }
	}
	int main(){
    ios::sync_with_stdio(0);
    cin>>n>>m;
    fro(i,1,n+m+1)
     root[i]=i;
    fro(i,1,n+1){
        fro(j,1,m+1){
            cin>>ch[i][j];
            if(ch[i][j]=='='){
                megeroot(i,j);
            }
        }
    }
    fro(i,1,n+1)
      fro(j,1,m+1){
        if(ch[i][j]=='=') continue;
        int x=findroot(i),y=findroot(j+n);
        if(ch[i][j]=='<'){
            edge[x].push_back(y);
            du[y]++;
        }
        else{
            edge[y].push_back(x);
            du[x]++;
        }
      }
    toolsort();
    //cout<<sum<<endl;
    //sum+=4;
    if(sum!=n+m)
        cout<<"No"<<endl;
    else{
            cout<<"Yes"<<endl;
        fro(i,1,n+1)
        cout<<answer[findroot(i)]<<" ";
        cout<<endl;
        fro(i,n+1,n+m+1){
            if(i!=n+m)
                cout<<answer[findroot(i)]<<" ";
            else
                cout<<answer[findroot(i)];
        }
        cout<<endl;
    }
    return 0;
	}



### F - Asya And Kittens 并查集+DFS
#### 题目大意
有n只小猫，开始将它们放在指定的n个单元格内，然后随机从n-1个隔板中拆除隔板，最终使得这些小猫在同一单元格。现在依次给出拆除隔板的顺序，比如：1 4 就表示1号和4号小猫之间的隔板会被拆除(注：只能拆除相邻区域小猫之间的隔板)<br>

严格的说：这是一类集合合并问题

#### 思路
按照顺序合并，当n-1次必须合并为一个集合，并查集解决。<br>
难点是记录数组，将两个集合合并。<br>
网上的解法很多，我用的是建树方法。<br>
因为最后都是一个集合，如果按照加入顺序中序遍历一下那个树就是结果。（待理论验证）

	vector<int> a[maxn];
	int root[maxn];
	int findroot(int i){
    return root[i]==i?i:root[i]=findroot(root[i]);
	}
	void mergeroot(int i,int j){
    int x=findroot(i);
    int y=findroot(j);
    if(x!=y){
        root[y]=x;
        a[x].push_back(y);
    }
	}
	void dfs(int i){
    cout<<i<<" ";
    fro(j,0,a[i].size()){
       dfs(a[i][j]);
    }
	}
	int main(){
    ios::sync_with_stdio(0);
    int n;
    cin>>n;
    fro(i,1,n+1)
    root[i]=i;
    fro(i,1,n){
        int x,y;
        cin>>x>>y;
        mergeroot(x,y);
    }
    dfs(findroot(1));
    return 0;
	}

