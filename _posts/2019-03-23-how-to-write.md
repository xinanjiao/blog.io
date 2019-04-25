---
layout: post
title: Function
date: 2019-4-21
categories: blog
tags: [算法,记忆化搜索]
description: 算法学习
---
### 题目描述


>对于一个递归函数w(a,b,c)
>如果  a≤0 or  b≤0 or 0  c≤0 就返回值 1.
>如果a>20 or b>20 or c>20就返回w(20,20,20).
>如果a<b并且b<c 就返回w(a,b,c−1)+w(a,b−1,c−1)−w(a,b−1,c).
>其它的情况就返回w(a-1,b,c)+w(a-1,b-1,c)+w(a-1,b,c-1)-w(a-1,b-1,c-1)w(a−1,b,c)+w(a−1,b−1,c)+w(a−1,b,c−1)−w(a−1,b−1,c−1)
>这是个简单的递归函数，但实现起来可能会有些问题。当a,b,c均为15时，调用的次数将非常的多。你要想个办法才行.

###### 特别的
比如 w(30,-1,0)既满足条件1又满足条件2
这种时候我们就按最上面的条件来算  所以答案为1

#### 输入输出格式

#### 输入格式
  若干行
  并以-1 -1 -1 结束
  保证输入的数在[-9223372036854775808,9223372036854775807]之间，并且是整数。

#### 输出格式
  输出若干行
  如
  >w(a,b,c)=ans;
  >>注意空格

#### 输入输出样例        
>输入样例              
>>1 1 1              
>>2 2 2              
>>-1 -1 -1
>输出样例
>>w(1 ,1 , 1)=2
>>w(2 ，2，2)=4

代码部分：


    #include <bits/stdc++.h>
    using namespace std;
    long long int p[25][25][25];//开辟数组储存每次计算的值
    long long int wabc(long long int a,long long int b,long long int c)

    {
    if(a<=0||b<=0||c<=0)
    {
        return 1;
    }

    else if(p[a][b][c]!=0)//如果遇到这个值计算过，直接返回
    {
        return p[a][b][c];
    }
    else if(a>20||b>20||c>20)
    {
         p[a][b][c]=wabc(20,20,20);
    }

    else if(a<b&&b<c)
    {
         p[a][b][c]=wabc(a,b,c-1)+wabc(a,b-1,c-1)-wabc(a,b-1,c);
    }
    else
       p[a][b][c]=wabc(a-1,b,c)+wabc(a-1,b-1,c)+wabc(a-1,b,c-1)-wabc(a-1,b-1,c-1);
       return p[a][b][c];
    }
    int main()
    {
    long long int a,b,c;
    memset(p,0,sizeof(long long int));
    while(1)
    {
        cin>>a>>b>>c;
        if(a==-1&&b==-1&&c==-1)
            break;
        cout<<"w("<<a<<", "<<b<<", "<<c<<") = ";
        if(a>20) a=21;
        if(b>20) b=21;
        if(c>20) c=21;
        cout<<wabc(a,b,c)<<endl;
    }
}







