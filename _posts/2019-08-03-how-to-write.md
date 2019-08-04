---
layout: post
title: ACM训练第12天
date: 2019-8-03
categories: blog
tags: [算法,构造+规律,训练]
description: 语言。
---

## 题目

### A - Odd Sum Segments 
题目链接：<https://vjudge.net/contest/316455#problem/A><br/>
You are given an array a consisting of n integers a1,a2,…,an. You want to split it into exactly k non-empty non-intersecting subsegments such that each subsegment has odd sum (i. e. for each subsegment, the sum of all elements that belong to this subsegment is odd). It is impossible to rearrange (shuffle) the elements of a given array. Each of the n elements of the array a must belong to exactly one of the k subsegments.

Let’s see some examples of dividing the array of length 5 into 3 subsegments (not necessarily with odd sums): [1,2,3,4,5] is the initial array, then all possible ways to divide it into 3 non-empty non-intersecting subsegments are described below:

[1],[2],[3,4,5];
[1],[2,3],[4,5];
[1],[2,3,4],[5];
[1,2],[3],[4,5];
[1,2],[3,4],[5];
[1,2,3],[4],[5].
Of course, it can be impossible to divide the initial array into exactly k subsegments in such a way that each of them will have odd sum of elements. In this case print “NO”. Otherwise, print “YES” and any possible division of the array. See the output format for the detailed explanation.

You have to answer q independent queries.

Input
The first line contains one integer q (1≤q≤2⋅105) — the number of queries. Then q queries follow.

The first line of the query contains two integers n and k (1≤k≤n≤2⋅105) — the number of elements in the array and the number of subsegments, respectively.

The second line of the query contains n integers a1,a2,…,an (1≤ai≤109), where ai is the i-th element of a.

It is guaranteed that the sum of n over all queries does not exceed 2⋅105 (∑n≤2⋅105).

Output
For each query, print the answer to it. If it is impossible to divide the initial array into exactly k subsegments in such a way that each of them will have odd sum of elements, print “NO” in the first line. Otherwise, print “YES” in the first line and any possible division of the array in the second line. The division can be represented as k integers r1, r2, …, rk such that 1≤r1< r2<⋯< rk=n, where rj is the right border of the j-th segment (the index of the last element that belongs to the j-th segment), so the array is divided into subsegments [1;r1],[r1+1;r2],[r2+1,r3],…,[rk−1+1,n]. Note that rk is always n but you should print it anyway.

Examples
inputCopy
3
5 3
7 18 3 14 1
5 4
1 2 3 4 5
6 2
1 2 8 4 10 2
outputCopy
YES
1 3 5
NO
NO

题目大意：给出一个数组判断把数组分为n个子数组，其中每个子数组里面的元素相加全是奇数，如果是输出yes在输出位置，不是则输出no。<br/>

题目思路：对于奇偶的问题, 要首先想到奇偶的交换律, 即
奇+奇 = 偶<br/>
偶+偶 = 偶<br/>
奇+偶 = 奇<br/>
这样的话, 我们考虑, 在奇数的个数>=k时, 当且仅当奇数的个数%2同k%2相同时, 即说明一定有解
原来碰到这种题的思路则是模拟，但往往这种题看起来很复杂，其实是有规律的，慢慢去想，去构造。

代码：(头文件没有给出)

    using namespace std;
    #define ll long long int
    #define fro(i,a,n) for(ll i=a;i<n;i++)
    #define pre(i,a,n) for(ll i=n-1;i>=a;i--)
    #define mem(a,b) memset(a,b,sizeof(a))
    typedef pair<int,int> P;
    const double PI = 3.1415926535897932;
    const double EPS=1e-6;
    const int maxn=1e5+10;
    const int INF=0x3f3f3f3f;
    int main()
    {
    ios::sync_with_stdio(false);
    int t;
    cin>>t;
    while(t--)
    {
        int m,n;
        cin>>m>>n;
        vector<int> a;
        int sum=0;
        fro(i,0,m)
        {
            int s;
            cin>>s;
            if(s%2!=0)
            {
                sum++;
                a.push_back(i+1);
            }
        }
        if(a.size()<n)
            cout<<"NO"<<endl;
        else if(sum>=n&&sum%2==n%2)
        {
            cout<<"YES"<<endl;
            fro(i,0,n-1)
            {
               cout<<a[i]<<" ";
            }
            cout<<m<<endl;
        }
        else
            cout<<"NO"<<endl;
    }
    }

### B - Nauuo and Chess 
题目链接:<https://vjudge.net/contest/316455#problem/B><br/>
题目大意:在一个 m×m 的棋盘上放 n 颗棋子，第 i 颗棋子的坐标为 (ri,ci)，需要满足 |ri−rj|+|ci−cj|≥|i−j|，求 m 的最小值以及任意一种摆放方案。依次输出m和每个棋子的(ri,ci)。<br/>


题目思路：构造，找规律，很多时候并不是需要output给出的输出，需要自己找<br>
如下图规律:

![规律](/img/cf1.jpg)

代码：（无头文件）

    using namespace std;
    #define ll long long int
    #define fro(i,a,n) for(ll i=a;i<n;i++)
    #define pre(i,a,n) for(ll i=n-1;i>=a;i--)
    #define mem(a,b) memset(a,b,sizeof(a))
    typedef pair<int,int> P;
    const double PI = 3.1415926535897932;
    const double EPS=1e-6;
    const int maxn=1e5+10;
    const int INF=0x3f3f3f3f;
    int main()
    {
    ios::sync_with_stdio(false);
    int n;
    cin>>n;
    int m=n/2+1;//推导可以得出
    cout<<m<<endl;
    fro(i,1,m+1)
    {
        cout<<1<<" "<<i<<endl;
        n--;
    }
    for(int i=2;i<=m&&n;i++)
    {
        cout<<i<<" "<<m<<endl;
        n--;
    }
    }

### Fibonacci Again
题目链接：<https://vjudge.net/contest/316455#problem/K><br/>
Problem Description
There are another kind of Fibonacci numbers: F(0) = 7, F(1) = 11, F(n) = F(n-1) + F(n-2) (n>=2).
 

Input
Input consists of a sequence of lines, each containing an integer n. (n < 1,000,000).
 

Output
Print the word "yes" if 3 divide evenly into F(n).

Print the word "no" if not.
 

Sample Input
0 1 2 3 4 5
 

Sample Output
no no yes no no no

题目大意：斐波那契数列我们很熟悉了，今天这道题也是，给出数组下标，叫你判断该数组下标对应的数是否能被3整除。<br/>
斐波那契数列到大概数组下标为44的时候连long long 都会溢出，更不用说1000000了，所以这也是一道规律题，先写一个暴力判断前面能被3整除的数的规律，进而得出答案，这里的规律是，小标除余4等于2的则为答案。（我空想了好久ORZ）<br/>

代码：

    using namespace std;
    #define ll long long int
    #define fro(i,a,n) for(ll i=a;i<n;i++)
    #define pre(i,a,n) for(ll i=n-1;i>=a;i--)
    #define mem(a,b) memset(a,b,sizeof(a))
    typedef pair<int,int> P;
    const double PI = 3.1415926535897932;
    const double EPS=1e-6;
    const int maxn=2e6+10;
    ll a[maxn];
    int main()
    {
    int n;
    while(cin>>n)
    {
        if(n%4==2)
            cout<<"yes"<<endl;
        else
            cout<<"no"<<endl;
    }
    }




