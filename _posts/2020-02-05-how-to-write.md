---
layout: post
title: Codeforces Round #541 (Div. 2)  UVA 1025
date: 2020-02-05
categories: blog
tags: [codeforce,动态规划]
description: 语言
---

## codeforce round 541 div2 持续更新

### A - Sea Battle
#### 题目大意
不好叙述，看图便知。就求两个矩形连在一起，周围的面积。<br>

#### 思路
刚开始一直想复杂了，最后想通了，不就一层是重合的吗

	int main(){
    ios::sync_with_stdio(0);
     int w1,h1,w2,h2;
     cin>>w1>>h1>>w2>>h2;
     int sum=(w1+2)*(h1+2)-(w1*h1);
     h2-=1;
     sum+=w2+2+(h2*2);
     sum-=w2;
     cout<<sum<<endl;
    return 0;
	}

### B - Draw!
#### 题目大意
给出每个时刻的比分，形如(a,b)。是a：b。问只给出这些，叫我们判断比分从0:0开始，有多少比分相等的情况。

#### 思路
唉，一开始思路对的，但细节没有处理好，居然去想暴力了，map不行还手写了hash表，最后肯定不行的。细节处理好就行了！

	P a[maxn];
	ll sum=0;
	void cheack(P a,P b){
    int maxa1=max(a.first,a.second);
    int maxa2=min(a.first,a.second);
    int maxb1=max(b.first,b.second);
    int maxb2=min(b.first,b.second);
    if(maxb2>=maxa1){
    sum+=maxb2-maxa1;
      if(maxa1>maxa2)
        sum+=1;
    }
	}
	int main(){
    ios::sync_with_stdio(0);
     int n;
     cin>>n;
     fro(i,0,n)
     cin>>a[i].fi>>a[i].se;
     vector<P> s;
     s.push_back(a[0]);
     int cnt=0;
     fro(i,1,n){
        if(a[i]!=s[cnt]){
          s.push_back(a[i]);
          cnt++;
        }
     }
     sum=1;
     P now=make_pair(0,0);
     int len=s.size()-1;
     fro(i,0,s.size()){
        P pos=s[i];
        cheack(now,pos);
        now=pos;
     }
     cout<<sum<<endl;
    return 0;
	}

### C - Birthday
#### 题目大意
n个数字，围成一圈，如何排序让两个之间的数字之差最小。

#### 思路
做的时候第一个感觉就和紫书上的贪心很像，往那边想了想，最大在中间，两边分别分配前面的数字。

	int a[110];
	int main(){
    ios::sync_with_stdio(0);
     int n;
     cin>>n;
     fro(i,0,n)
     cin>>a[i];
     sort(a,a+n);
     map<int,int> mp;
     int i=0;
     while(i<n){
        mp[i]=1;
        cout<<a[i]<<" ";
        i+=2;
     }
     for(int i=n-1;i>=0;i--){
        if(!mp[i]){
            if(i==1)
                cout<<a[i];
            else
                cout<<a[i]<<" ";
        }
     }
     cout<<endl;
    return 0;
	}



### uva 1025 城市里的间谍 多决策动态规划 固定终点的DAG

#### 题目大意
给定n个车站，时间T，列车从车站i到车站i+1需要的时间t[i]。给定M1辆从车站1驶往车站n的列车发车时间，以及M2辆从车站n驶往车站1的列车发车时间。皮皮怪初始在车站1，要在T时刻到达车站n，问他至少要在车站等多久——车在车站停留时间忽略不计，并且假设皮皮怪特别敏捷，两辆车同时到车站他也能完成换乘，如果T时刻不可能在车站n就输出impossible。

#### 思路
这是一个很明显的多决策的问题，假设皮皮 i 时刻在车站 j（以dp[i][j]表示从现在开始他需要在车站等车的最少时间），那么他有三个决策：

(1)原地等一秒：dp[i][j] = dp[i+1][j]

(2)如果有向左的列车可搭乘：dp[i][j] = dp[i+t[i-1]][j-1]

(3)如果有向右的列车可搭乘：dp[i][j] = dp[i+t[i]][j+1]

我们需要做的就是在三个决策寻找最优的，也就是三个里最小的~


啊啊啊啊，完全没思路，我要学的还很多啊！！！

	int dp[1020][1000];
	bool hash_train[1000][1060][2];
	int cost[1160];
	int main(){
    ios::sync_with_stdio(0);
     int n,case1=1;
     ofstream out;
     out.open("output.txt");
     while(cin>>n&&n){
        int t;
        cin>>t;
        mem(hash_train,0);
        mem(dp,0);
        mem(cost,0);
        fro(i,1,n){
            cin>>cost[i];
        }
        int M1,M2;
        cin>>M1;//向右开
        fro(i,0,M1){
            int a;
            cin>>a;
            hash_train[a][1][0]=1;
            int sum=a;
            fro(j,2,n+1){
                sum+=cost[j-1];
                hash_train[sum][j][0]=1;
            }
        }
        cin>>M2;//向左开
        fro(i,0,M2){
            int a;
            cin>>a;
            hash_train[a][n][1]=1;
            int sum=a;
            for(int j=n-1;j>=1;j--){
                sum+=cost[j];
                hash_train[sum][j][1]=1;
            }
        }
        fro(i,1,n) dp[t][i]=INF;
        dp[t][n]=0;
        for(int i=t-1;i>=0;i--){
            for(int j=1;j<=n;j++){
                dp[i][j]=dp[i+1][j]+1;
                //往右
                if(j<n&&hash_train[i][j][0]&&i+cost[j]<=t)
                    dp[i][j]=min(dp[i][j],dp[i+cost[j]][j+1]);
                //往左
                if(j>1&&hash_train[i][j][1]&&i+cost[j-1]<=t)
                    dp[i][j]=min(dp[i][j],dp[i+cost[j-1]][j-1]);
            }
        }
        cout<<"Case Number "<<case1++<<": ";
        if(dp[0][1]>=INF)
            cout<<"impossible"<<endl;
        else
            cout<<dp[0][1]<<endl;
        }
        out.close();
    return 0;
	}




