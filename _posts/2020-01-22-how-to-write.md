---
layout: post
title: uva 10507 贪心暴力
date: 2020-1-22
categories: blog
tags: [贪心]
description: 语言
---
### 啊
开始走亲访友了，一玩几天几天的，太难了，我也不想到处走啊，python还挺在网页爬取那儿，如果电脑带去又觉得没时间看，害！既然不得不玩就好好玩玩吧还是。。。。。

### uva 10507 暴力

#### 题目大意
给出一个序列，可以交换两个数字的位置，问最少交换几次，可以使序列有序（升序或者降序，可成环，如２３４１也算）,(1<=n<=500)

#### 思路
枚举第一个数为1到n，暴力交换即可，但我不知道我的交换有啥子毛病，一直wa，最后还是数组模拟过了，时间复杂度O(n * n)




    int main(){
    ios::sync_with_stdio(0);
    int n;
    //ofstream out;
    //out.open("output.txt");
    while(cin>>n&&n){
        int a[510],b[510],c[510],d[510];
        map<int,int> mp;
        fro(i,1,n+1){cin>>a[i];
        //mp[a[i]]=i;
        c[a[i]]=i;
        }
        int l=1,mina=INF;
        while(l<=n){
           int pos=1,ans=l;
           int sum=0;
        memcpy(b,a,sizeof(a));
        //map<int,int> mp2(mp);
        memcpy(d,c,sizeof(c));
        while(pos<=n){//逆时针
          int t=ans;
          if(b[pos]!=t){
            sum++;
            b[d[t]]=b[pos];
            d[b[pos]]=d[t];
            b[pos]=t;
            d[t]=pos;
            //swap(b[mp2[t]],b[pos]);
            //mp2[b[pos]]=mp2[t];
            //mp2[t]=pos;
          }
          ans++;
          if(ans>n)
            ans=1;
          pos++;
        }
        mina=min(mina,sum);
        if(mina==0){
            break;
        }
        memcpy(b,a,sizeof(a));
        //map<int,int> mp3(mp);
        memcpy(d,c,sizeof(c));
        pos=1,ans=l;
        sum=0;
        while(pos<=n){//顺时针
            int t=ans;
            if(b[pos]!=t){
                sum++;
            b[d[t]]=b[pos];
            d[b[pos]]=d[t];
            b[pos]=t;
            d[t]=pos;
            }
            ans--;
            if(ans==0)
            ans=n;
            pos++;
        }
        mina=min(mina,sum);
        if(mina==0)
            break;
        l++;
        }
        cout<<mina<<endl;
    }
    //out.close();
    return 0;
    }









