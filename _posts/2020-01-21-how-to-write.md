---
layout: post
title: uva 1153 贪心优先队列
date: 2020-1-21
categories: blog
tags: [贪心]
description: 语言
---

## 最近
<p style="color:red">我对非典没有啥子印象，那时还小。但最近新型的冠状病毒又爆发，还是临近新年之际，这无疑给合家团聚的人们心里蒙上了一层灰蒙蒙的担心。最近全国22个省市相继报道了确诊病例。，从你得到红包了吗，变为你买到口罩了吗。这可能是中国下一个非常时期，但只要我们相信奔赴在前线的科学工作者们，没有什么不可战胜的！！！！</p>

### uva 1153 贪心+优先队列

#### 题目大意
有n个工作(1<=n<=800000)，分别给出该工作的需要完成的时间和截至时间。且工作只能串行完成（即一个个的完成），问最多能完成几个工作。

#### 思路
很像区间的贪心，区间的贪心知道首尾，但这道题不知道首尾，只知道尾，可谓“神龙见尾不见首”。<br>
贪心思路:<br>
1. 按截至时间排序
2. 遍历数组，在保证能加入到该完成的序列中，就加进去。如果不能加进去，就比较已完成的序列中花费时间最多的那个工作，并替换掉。（因为要保证为后面的工作留下足够的时间）。

code

    struct node{
    int over;
    int cost;
    node(){}
    };
    node a[maxn];
    bool cmp(node a,node b){
    if(a.over==b.over)
        return a.cost<b.cost;
    else
        return a.over<b.over;
    }
    int main(){
    ios::sync_with_stdio(0);
    int t;
    cin>>t;
    while(t--){
        mem(a,0);
        int n;
        cin>>n;
        fro(i,0,n){
            cin>>a[i].cost>>a[i].over;
        }
        sort(a,a+n,cmp);
        int sum=1;
        int l=0;
        priority_queue<int> p;
        fro(i,0,n){
            if(a[i].cost+l<=a[i].over){
                p.push(a[i].cost);
                l+=a[i].cost;
            }
            else if(!p.empty()){
                int s=p.top();
                if(s>a[i].cost){
                    l-=s;
                    l+=a[i].cost;
                    p.pop();
                    p.push(a[i].cost);
                }
            }
        }
      cout<<p.size()<<endl;
      if(t)cout<<endl;
    }
    return 0;
    }












