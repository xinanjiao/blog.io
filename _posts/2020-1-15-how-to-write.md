---
layout: post
title: uva1149（贪心+中途相遇法思想）12145(思维)
date: 2020-1-15
categories: blog
tags: [贪心,中途相遇法]
description: 语言。
---
贪心的题，真的榨干了我仅有的智商......太难顶了:(

### Bin Packing UVA - 1149 贪心+中途相遇法

#### 题目大意
给定N（n<=10e5）个物品的重量Li,背包的容量为M，同时要求每个背包最多装两个物品，求至少要多少个背包。

#### 思路
思路一：<br>
从大到小排序，从第一个开始，除掉这个数找后面n-i个数中第一个小于等于M-L的那个数，将那个数标记。复杂度nlogn。苦于对upper_bound和lower_bound的捉摸不透，放弃了。<br>
思路二：<br>
收尾开始匹配，像中途相遇法一样一个个看和是否大于M。具体看代码：

    int a[maxn];
    void init(int n){
    fro(i,0,n)a[i]=0;
    }
    int main(){
    ios::sync_with_stdio(0);
    int t;
    cin>>t;
    while(t--){
       
        int n,maxl;
        cin>>n;
        init(n);
        cin>>maxl;
        fro(i,0,n)cin>>a[i];
        int sum=0;
        sort(a,a+n,greater<int>());
        int l=0,r=n-1;
        while(l<=r){
            if(a[l]+a[r]<=maxl){
                l++;
                r--;
            }
            else
                l++;
               sum++;
        }
        cout<<sum<<endl;
    if(t)cout<<endl;
    }
    //out.close();
    return 0;
    }

### Bits Equalizer UVA - 12545思维题
#### 题目大意
输入两个等长的（长度不超过100）的串S和T，其中S包括字符0，1，？。但T只包括0，1。你的任务是用尽量少的步数把S变为T。每步有3种操作：<br>
1. 把S中的0变为1
2. 把S中的？变为0或1
3. 交换S中的任意两个字符

#### 思考
关于这种类型的题我见到好几次了，要么是规律，要么暴力，我也刻意往规律方向想了想，但脑子总是越想越乱，没个方向，像无头苍蝇一样乱撞，看了题解，学到了！<br>

实际上这样考虑是最好的：

因为？的反正都可以变，因此放哪儿都无所谓，真正对我们有影响的是交换的这一步

首先，假若第一个串的1比第二个串的1多，肯定无解，因为1是不能变成其他的元素的。

其次，我们先尽可能的交换，让0和0匹配，1和1匹配，剩下的？只变换就好了。

那为了做到尽可能的交换，我们需要记录4个值(已经匹配的字符我们就不管了)

第二个串的0对应第一个串的1的数目，a

第二个串的0对应第一个串的？的数目，b

第二个串的1对应第一个串的0的数目，c

第二个串的1对应第一个串？的数目，d



首先，考虑a==c的情况，那么此时做a次交换，b+d次变换即达到要求，一共步数是a+b+d

然后考虑a>c的情况，首先说明a是不可能大于c+d的，不然就是无解

那么我们首先做a次交换(其中c次是和0交换，a-c次是和？交换)，把1先尽可能的匹配好，此时串里还剩下b+d个问号待变换，因此一共的步数也是a+b+d

然后考虑a < c的情况

此时我们做a次交换，把1匹配好，此时串里还剩下b+d个问号，c-a个0等待变成1，所以一共的步数是a+c-a+b+d -> b+c+d



    int main(){
    ios::sync_with_stdio(0);
    int t,case1=1;
    cin>>t;
    while(t--){
        string s1,s2;
        cin>>s1>>s2;
        int a=0,b=0,c=0,d=0;
        //a为s2中0对应的1
        //b为s2中0对应的？
        //c为s2中1对应的0
        //d为s2中1对应的?
        cout<<"Case "<<case1++<<": ";
        if(s1.size()!=s2.size())
        cout<<-1<<endl;
        else{
            int len=s2.size();
            fro(i,0,len){
                if(s2[i]!=s1[i]){
                    if(s2[i]=='0'){
                        if(s1[i]=='1')
                            a++;
                        else
                            b++;
                    }
                    else{
                        if(s1[i]=='0')
                            c++;
                        else
                        d++;
                    }
                }
            }
                if(a>c+d)
                    cout<<-1<<endl;
                else{
                    if(a==c)
                        cout<<a+b+d<<endl;
                    else if(a>c)
                        cout<<a+b+d<<endl;
                    else
                        cout<<c+b+d<<endl;
                }
        }
    }
    return 0;
    }







