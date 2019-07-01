---
layout: post
title: 木块vector数组
date: 2019-05-06
categories: blog
tags: [算法,uva]
description: 文章金句。
---
## 题目描述
输入n，得到编号为0~n-1的木块，分别摆放在顺序排列编号为0~n-1的位置。现对这些木块进行操作，操作分为四种。

1、move a onto b：把木块a、b上的木块放回各自的原位，再把a放到b上；

2、move a over b：把a上的木块放回各自的原位，再把a发到含b的堆上；

3、pile a onto b：把b上的木块放回各自的原位，再把a连同a上的木块移到b上；

4、pile a over b：把a连同a上木块移到含b的堆上。

当输入quit时，结束操作并输出0到n-1的位置上的木块情况

### sample input
10<br/>
move 9 onto 1<br/>
move 8 over 1<br/>
move 7 over 1<br/>
move 6 over 1<br/>
pile 8 over 6<br/>
pile 8 over 5<br/>
move 2 over 1<br/>
move 4 over 9<br/>
quit<br/>
### sample output
 0: 0<br/>
 1: 1 9 2 4<br/>
 2:<br/>
 3: 3<br/>
 4:<br/>
 5: 5 8 7 6<br/>
 6:<br/>
 7:<br/>
 8:<br/>
 9:<br/>

### 想法
没有学c++之前，你会怎么想，创一个一维数组和一个二维数组，模拟上述四步操作，最后分别输出0到9中的数字？这种方法我看行，但是c++STL提供了一种更简单便捷的操作方式,叫容器，这个vector就是顺序容器之一，后面还有关联容器。
vector是一个向量容器，它可以像数组一样存储数据，所以我觉得他也算半个数组叭，它可以通过下标访问元素，为什么说他好呢？
1. 他可以自由压缩空间，而且通过你读入的数据进行分配空间，也就是说，你可以自由删除和添加元素，而不用在进行其他操作。
2. vector容器包括定义赋值，插入，删除，显示等操作，定义和显示需要着重理解一下，而其他则由具体的实现函数实现。

vector操作：<br/>
定义：vector<类型> a;//定义空的元素<br/>
赋值:c1=c2//c2元素全部赋值给c1<br/>
访问：<br/>
1. c.at[n]//直接访问n所标识的元素，若下标越界返回"out_of_range";<br/>
2. c[n]//访问n的元素，没有at安全<br/>
3. c.back()//返回最后一个元素<br/>
其它操作：<br/>
c.push_back(n)//尾部插入n元素<br/>
c.insert(pos,n)//在pos位置插入e元素，并返回新元素的位置<br/>
c.pop_back()//删除最后一个元素<br/>
c.erase(pos)//删除pos位置的元素<br/>
c.clear()//删除所有元素<br/>
c.size()//返回元素个数<br/>
c.rsize(n)//重新定义元素个数.<br/>

行了，都准备就绪了，那就开始吧！<br/>

    #include<bits/stdc++.h>
    using namespace std;
    int n;
    vector<int> pile[30];//vector容器
     void find_block(int a,int &p,int &h)
    {
    for(p=0;p<n;p++)
        for(h=0;h<pile[p].size();h++)
        if(pile[p][h]==a)return ;
    }
    void clear_above(int a,int h)
    {
    for(int i=h+1;i<pile[a].size();i++)
    {
        int b=pile[a][i];
        pile[b].push_back(b);//归位上面的元素
    }
    pile[a].resize(h+1);
    }
     void moveonf(int a,int ha,int b)
    {
    for(int i=ha;i<pile[a].size();i++)
    {
        pile[b].push_back(pile[a][i]);
    }
    pile[a].resize(ha);
    }
    void printf()
    {
    for(int i=0;i<n;i++)
    {
        cout<<i<<":";
        for(int j=0;j<pile[i].size();j++)
            cout<<" "<<pile[i][j];
        cout<<endl;
    }
    }
    int main()
    {
    string s1,s2;
    int a,b;
    cin>>n;
    for(int i=0;i<n;i++)
        pile[i].push_back(i);//push_back()在数组尾部添加元素
    while(cin>>s1&&s1!="quit")
    {
        cin>>a>>s2>>b;
        int pa,ha,pb,hb;
        find_block(a,pa,ha);//pa为a所在的堆数，ha为a所在该堆的高度（位置）
        find_block(b,pb,hb);
        if(pa==pb)continue;
        if(s2=="onto")
            clear_above(pb,hb);
        if(s1=="move")
            clear_above(pa,ha);
        moveonf(pa,ha,pb);
    }
    printf();
    }


上面四种操作并不是真的要四种，其实有三种，有onto把b上方木块全部归位，有over把a上方全部归位，最后在一起把a放在b上面。这次它定义了一个二维vector数组，也就表示二维数组了，但是删除移动都很方便，我觉得很好。

### 反正一直冲就完事儿了，加油








