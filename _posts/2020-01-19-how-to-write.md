---
layout: post
title: uva 1613暴力搜索+1614 贪心 结论
date: 2020-1-19
categories: blog
tags: [深度搜索,贪心]
description: 语言
---

### K-Graph Oddity UVA - 1613 暴力搜索
#### 题目大意
染色问题，输入一个一个n(3<=n<=9999)个点m条边的连通图，n保证是奇数，设k为最小的奇数，使得每一个点的度数不超过它，你的任务是把图中的结点涂上颜色1到k，使得相邻点的颜色不同。多解时输出任意解。输入保证有解。

#### 思路
dfs暴力搜索。刚开始思路是：从第一个点开始找找，然后遍历它链接的所有的点，看那个颜色没有被涂过。dfs要保证每个点都遍历到，但这个代码有问题后者有例子我没有想到，交上去超时了。<br>

后来，搜索方式变为如果连接的点哪个没被染色，就搜索那个点，但是.....wa了！！！最后找到bug居然是初始化时只到n-1，应该是到n的。最后时间为1350ms,比起那些100ms的代码来说，已经很慢了。。。。可能没关闭同步流

    int color[maxn];
    int book[maxn];
    int book2[maxn];
    vector<int> s[maxn];
    int tool[maxn];
    int n,m,k;
    void init(int n){
    fro(i,0,n+3){
    tool[i]=0;
    color[i]=0;
    s[i].clear();
    }
    }
    void dfs(int x,int id){
    //book[x]=1;
    for(int i=0;i<s[x].size();i++){
        if(color[s[x][i]])
            book2[color[s[x][i]]]=1;
    }
    fro(i,1,k+1){
        if(!book2[i]){
            color[x]=i;
            //cout<<x<<" "<<i<<endl;
            break;
        }
    }
    mem(book2,0);
    fro(i,0,s[x].size()){
        if(!color[s[x][i]]){
            dfs(s[x][i],id+1);
        }
    }
    }
    int main(){
    int aa=1;
    while(cin>>n>>m){
        init(n);
        int maxa=-1;
        fro(i,0,m){
            int a,b;
            cin>>a>>b;
            s[a].push_back(b);
            s[b].push_back(a);
            tool[a]++;
            maxa=max(maxa,tool[a]);
            tool[b]++;
            maxa=max(maxa,tool[b]);
        }
        k=maxa%2==0?maxa+1:maxa;
        dfs(1,1);
        if(aa!=1)
            cout<<endl;
            aa=2;
        cout<<k<<endl;
        fro(i,1,n+1)
        cout<<color[i]<<endl;
    }
    return 0;
    }











