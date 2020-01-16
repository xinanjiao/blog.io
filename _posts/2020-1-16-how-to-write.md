---
layout: post
title: uva 1610+11491 (贪心)
date: 2020-1-16
categories: blog
tags: [算法,贪心]
description: 语言
---

贪心，真的很贪心....吐老血了。。。。做下来脑袋回响起一连串声音“我为什么没想到” “我为什么没想到”“我为什么没想到”.......

### Party Games UVA - 1610 贪心+思维+细节
#### 题目大意
细节题。给出偶数个字符串，你应当给出一个字符串大于给出字符串的一半，和小于一半。

#### 思考
简单吧。嘿嘿。。。。简单个屁！！！！看题面知道是那么一回事，自己动手一敲，害！细节很多，有些还不怎么好想，我照着debug一通乱改。debug全过了，但交上去过不了，这时代码已被改得又臭又长，无心在维护它了，故弃之。网上观大佬之code，简短意赅，精巧思路明了，还很快（基本上没耗时）。我还是思想没到位！！！！

我们设我们要比较的两个字符串分别为l和r。l < r。  实际上我们要找的字符串的长度不可能大于r，不然就大于r了。但也可以不能长度长于l，因为题目有“如果有多组解，输出长度最小且字典序最小的字符串”。所以我们直接用l的长度做参考。

需要注意的几个点：当字母为'Z'时，不能运算，要保留且继续遍历l。不断构造字符串的字母看是否符合小于R，满足就输出。


    int main(){
    ios::sync_with_stdio(0);
    int n;
    while(cin>>n&&n){
        string s[1010];
        fro(i,0,n){
        cin>>s[i];
        }
        sort(s,s+n);
        int a=n/2,b;
        b=a-1;
        string s1=s[b],s2=s[a];
        string ans="";
        int len=s1.size()-1;
        for(int i=0;i<=len;i++){
            if(i<len&&s1[i]!='Z'){
                ans+=s1[i]+1;
                if(ans<s2)break;
                //cout<<ans[i]<<endl;
                ans[i]--;
            }
            else
                ans+=s1[i];
        }
        cout<<ans<<endl;
    }
    return 0;
    }


### Erasing and Winning UVA - 11491 贪心
#### 题目大意
给出一个长度为n的数字序列，你需要删掉d个数字，但要保证最后的数值尽可能的大。(1 <= d < n <1e5)。

#### 题解
赤裸裸的贪心。贪心的使从高位开始依次最大，这样就能保证最后的结果最大

    char ans[maxn];
    int main(){
    ios::sync_with_stdio(0);
    int n,m;
    while(scanf("%d %d",&n,&m)&&(n+m)){
        getchar();
        int cnt=0,s=m;
        fro(i,0,n){
            char ch=getchar();
            //cout<<ch<<endl;
            while(m&&ch>ans[cnt-1]&&cnt>0){
                cnt--;
                m--;
            }
            ans[cnt++]=ch;
        }
        ans[n-s]=0;
        cout<<ans<<endl;
    }
    return 0;
    }









