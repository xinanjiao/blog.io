---
layout: post
title: POJ之哈希专题（数字哈希续）
date: 2020-02-23
categories: blog
tags: [数字哈希]
description: 文章金句。
---

### 关于这个专题
hash专题误入概率dp，直接看懵，看网上分析才知道不是hash!<br>

我也确实做到了几道经典题，但今天也有一些数据结构能水过的题，字符串hash我觉得我还没遇到真正的。<br>

上学期数据结构学hash表的时候就觉得很难，今天做到这个专题我也没有去专研，但学到了很多有用的快速查找方法。<br>

做题也遇到很多盲区，我希望知道后能一遍一遍的复习，下次遇见的时候也要记住！<br>

另外，中国加油啊！


### Eqs POJ - 1840  单个数字hash

#### 题目大意
考虑这个等式: 
a1x1 3+ a2x2 3+ a3x3 3+ a4x4 3+ a5x5 3=0 
系数a1,a2,a3,a4,a5是都是整数，范围是[-50,50]. 
考虑一组解 (x1, x2, x3, x4, x5) ,并且 xi∈[-50,50], xi != 0 

全输出有多少组这样的解 

#### 思路
暴力5个循环直接告退。<br>
和紫书上的一道例题类似，逐步降低复杂度。<br>
先算出全部a1和a2的关于x的值，最后枚举a3,a4,a5的关于x的值，在hash查找看后者乘以-1能不能在hash表中找到。<br>
用map超时无疑，因为复杂度的常数很大<br>

有个坑点就是，hash表不能只光判断是否出现过，还要记录次数，比如n,m为值sum，但m,n也为值sum时就可能漏算，所以保留的是次数。<br>

<p style="color: red">刚开始发现没有限定x是否为整数，我就懵逼了，实际上发现如果不为整数的话，根本不能枚举！</p>

<p style="color: red;font-size: 24px;">值得注意的是：交了一发hash模板MLE了。为什么会MLE，因为内存超限，看下面的hash模板</p>

```
const int hashmaxn=8388608;
int lowbit(int x){return x&(-x);}
struct hash_map{
    static const int maxa=0x7fffff;
    int q[hashmaxn];
    int p[hashmaxn];
    void clear(){
        for(int i=0;i<maxa;i++)
            q[i]=0;
    }
    short &operator[](int k){
        int i;
        for(i=k&maxa;q[i]&&p[i]!=k;i=(i+1)&maxa);
        p[i]=k;
        return q[i];
    }
}mp;

```

今天我终于知道为什么hashmaxn是那一串数字了。poj的内存限制是Memory limit 65536 kB。我上网搜了一下这样的内存可以开多大的数组，结果是运行这个程序便知
```
65536*1024/sizeof(int)
```
最后结果是8388608的两倍，这解释了为什么hash模板里面有两个数组且长度为这么大。因为刚好利用完空间，把空间利用到极致最后节约时间！！

好，解释一下为什么会MLE。<br>


int的字节为4字节，在这道题中，可能会溢出。造成MTL。解决方法有两种<br>

换为char型或者short型，char型不做考虑，重点**short型**<br>

short型接触较少，c语言初学的时候知道，但不经常用。**short两字节**，表示数的范围为范围**-32768 ~ +32767**。因为考虑本题次数不会很大，且short字节更短，所以用short表示q数组的次数，p数组表示值，依然用int数组。

这样一改就可以过啦！！969ms 4万多kb，还是有MLE的风险....

```
const int hashmaxn=8388608;
int lowbit(int x){return x&(-x);}
struct hash_map{
    static const int maxa=0x7fffff;
    short q[hashmaxn];
    int p[hashmaxn];
    void clear(){
        for(int i=0;i<maxa;i++)
            q[i]=0;
    }
    short &operator[](int k){
        int i;
        for(i=k&maxa;q[i]&&p[i]!=k;i=(i+1)&maxa);
        p[i]=k;
        return q[i];
    }
}mp;
int main()
{
    ios::sync_with_stdio(0);
    ll sum=0,ans=0;
    ll a1,a2,a3,a4,a5;;
    mp.clear();
    //map<ll,int> mp;
    cin>>a1>>a2>>a3>>a4>>a5;
    for(ll i=-50;i<=50;i++){
            if(i==0)
            continue;
        for(ll j=-50;j<=50;j++){
                if(j==0)
                continue;
                sum=a1*i*i*i+a2*j*j*j;
                //sum*=-1;
                mp[sum]++;
        }
    }
    for(ll i=-50;i<=50;i++){
        if(!i) continue;
        fro(j,-50,51){
            if(!j) continue;
            fro(k,-50,51){
                if(!k) continue;
              sum=a3*i*i*i+a4*j*j*j+a5*k*k*k;
              sum*=-1;
            //  cout<<sum<<endl;
              if(mp[sum])
                ans+=mp[sum];
            }
        }
    }
    cout<<ans<<endl;
    return 0;
}
```



