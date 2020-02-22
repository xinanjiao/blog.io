---
layout: post
title: POJ专题之哈希（数字哈希）
date: 2020-02-22
categories: blog
tags: [数字哈希]
description: 文章金句。
---

### 哈希表
哈希表的运用可以大大减轻时间复杂度，是一个典型的空间换时间的例子。熟知的STL函数map也可以保存上一个状态，但在一些题目中，由于常数巨大，导致map会超时，一般需要自己手写hash表。但hash表没有固定的格式，需要不断的去了解。<br>

哈希的poj经典题目挺多，今天接触了两道题，关于数字哈希的，原来背了一个关于处理一个数的hash模板，但在这两个题都用不到，但我也学会了储存连续的数的状态hash，**用vector<int> a[maxn],maxn存储他们的和，但总和要除余hash表长，vector保存该一串数字的状态**，相比其他形式的hash，简单易懂还很好理解。


### Snowflake Snow Snowflakes POJ - 3349 数字hash

#### 题目大意
在n （n<100000)个雪花中判断是否存在两片完全相同的雪花，每片雪花有6个角,每个角的长度限制为1000000

 两片雪花相等的条件：

雪花6个角的长度按顺序相等（这个顺序即可以是顺时针的也可以是逆时针的）

#### 思路
思路为：边输入边保存前面的状态，分别通过顺时针和逆时针去和原来的串比对是否符合。<br>
这就要储存原来的一串数字的状态，这就是上面的思路：**将一串数通过和来hash，通过和来代表这一串数，每次判断和来对比和相等的串**。

ps:这道题时间卡得非常紧，也就是数据很大，先前我用了数据结构TLE差不多一页吧（狗头）。最后用这个方法3909ms水过（4000ms限制）。还有就是数据结构(STL)虽然简单，但卡常数很大的数据真的很费时间。听说将vector换为普通数组更快！

```
const int maxn = 1e5+10;
const int hashmaxn=8388608;
int lowbit(int x){return x&(-x);}
const int p=1e6+100;
int s[maxn][6];
vector<int> a[p];
bool flag=0;
bool judge(int x,int y){
    //将y与x比对
    //顺时针
    for(int i=0;i<6;i++){
        int l=0,r=i;
        while(l<6){
            if(s[x][l]!=s[y][r])
                break;
            l++;
            r++;
            if(r==6) r=0;
        }
        if(l==6){
            flag=1;
            return true;
        }
    }
    //逆时针
    for(int j=0;j<6;j++){
        int l=0,r=j;
        while(l<6){
            if(s[x][l]!=s[y][r])
                break;
            l++;
            r--;
            if(r<0)
                r=5;
        }
        if(l==6){
            flag=1;
            return 1;
        }
    }
    return 0;
}
int main()
{
    ios::sync_with_stdio(false);
    int n;
    scanf("%d",&n);
    fro(i,0,n){
        int sum=0;
        fro(j,0,6){
            scanf("%d",&s[i][j]);
            sum+=s[i][j];
            sum%=p;
        }
        if(flag) continue;
        if(!a[sum].empty()){
            for(int k=0;k<a[sum].size();k++){
                if(judge(a[sum][k],i))
                    break;
            }
        }
        a[sum].push_back(i);
    }
    if(flag)
        printf("Twin snowflakes found.\n");
    else{
        printf("No two snowflakes are alike.\n");
    }
    return 0;
}
```


### Gold Balanced Lineup POJ - 3274 数字hash
#### 题目大意
给出每头牛所拥有的特征，比如某头牛特征值是13，13的二进制为1101，那么这头牛拥有特征1、3、4。 
现在给出牛的个数n（1<=n<=100000)和总共特征个数k(1<=k<=30)，求最长的区间，使得区间内所有牛的k个特征相加之和都相等。 

#### 思路（很经典的一道题）
的确想不到hash还可以用到这个地方，晃眼一看，真的看不出是hash，因为还要经过转化。<br>
暴力看数据就不可行。<br>
所以就要转化问题。

首先是题意的转换，怎么去寻找最大的距离

那么我们看例子

```
7 3
7
6
7
2
1
4
2
```
转化为2进制
```
111

110

111

010

001

100

010
```
逐个累加
```
111

221

332

342

343

443

453
```
分别减去最右边的数
```
000

110《----

110

120

010
110《----

120
```
就可以发现是2和6是最远的相同数字，那么只用6-2=4即可算出答案

 

那到底为什么这么做就对了呢？

为什么两个数一样就得到了正确解了呢。

可以这么理解，**当两个数相同时，说明在这两个数之间出现的数在每个特征上出现的数目相同，否则是不会两个数相同的。因为只有最右边的那个数增加了和左边所有的数增加的数字相同，他们才会减去最右边的数，出现相同**

对于刚开始必须要有0000，因为没有这个初始的0000，那么第一个牛就算不进去。（因为这个又贡献一页的wa(ㄒoㄒ))

减也不一定都要减第一个数，可以任意一个数！

后面就又是我们上面的数字的hash方法啦！

最后时间929ms(1000ms限制)

```
const int maxn = 1e6+10;
const int hashmaxn=8388608;
int lowbit(int x){return x&(-x);}
const int p=1e5+10;
int sum[maxn][33];
vector<int> hash_a[maxn];
int n,k,m;
int maxa=0;
bool check(int a,int b){
    fro(i,0,k){
        if(sum[a][i]!=sum[b][i])
            return 0;
    }
    //cout<<b<<" "<<a<<endl;
    maxa=max(maxa,b-a);
    return 1;
}
int main()
{
    ios::sync_with_stdio(0);
    cin>>n>>k;
    fro(i,1,n+1){
        cin>>m;
        fro(j,0,k){
            sum[i][j]=sum[i-1][j];
            if(m&(1<<j))
                sum[i][j]++;
        }
    }
    hash_a[0].push_back(0);
    fro(i,0,n+1){
        fro(j,1,k){
            sum[i][j]-=sum[i][0];
        }
        sum[i][0]=0;
        int ans=0;
        fro(j,0,k){
         ans+=sum[i][j];
         }
         ans=(abs(ans)%p);
        if(!hash_a[ans].empty()){
            for(int j=0;j<hash_a[ans].size();j++){
                if(check(hash_a[ans][j],i)){
                    break;
                }
            }
        }
        hash_a[ans].push_back(i);
    }
    cout<<maxa<<endl;
    return 0;
}
```
