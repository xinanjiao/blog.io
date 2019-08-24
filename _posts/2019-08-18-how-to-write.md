---
layout: post
title: ACM训练第20天（带权并查集）
date: 2019-8-18
categories: blog
tags: [算法,训练,数据结构,数学,图论]
description: 文章金句。
---

### 写在前面
送给自己两个字：加油!

## 题目
今天了解了带权（种类）并查集，图论中的最短路径算法，复习了拓扑排序，做了一道数学题。
带权并查集博客:<https://blog.csdn.net/yjr3426619/article/details/82315133><br/>
带权并查集入门：<https://blog.csdn.net/qq_42505741/article/details/81116572><br/>
做题总结：种类并查集是带权并查集的变种，按理说可以用带权并查集做，带权并查集和普通并查集的区别是：多了一个数组维护权值，每次路径压缩时，都要维护权值，当然维护权值的数组还可以表示种类，gay虫用1和0表示同类和异类，poj1703也通过类似的方法区别是否一组，1703如何判断不确定？如果不在一个集合，那么关系就是不确定的


### hdu 3038 带权并查集 How Many Answers Are Wrong 
题目链接：<https://vjudge.net/problem/HDU-3038><br/>
有M个数，不知道它们具体的值，但是知道某两个数之间（包括这两个数）的所有数之和，现在给出N个这样的区间和信息，需要判断有多少个这样的区间和与前边已知的区间和存在矛盾。例如给出区间和[1,4]为20，[3,4]为15，再给出[1,2]为30，显然这个[1,2]的值就有问题，它应该为20-15=5。注意为开区间。<br/>

    int root[maxn];
    int value[maxn];
    int ans=0;
    int findroot(int a)
    {
    if(a!=root[a])
    {
        int t=root[a];
        root[a]=findroot(root[a]);
        //路径压缩
        value[a]+=value[t];
    }
    return root[a];
    }
    void unionroot(int a,int b,int va)
    {
    int aa=findroot(a);
    int bb=findroot(b);
    if(aa!=bb)
    {
        root[aa]=bb;
        value[aa]=value[b]+va-value[a];
    }
    else
    {
        int k=abs(value[a]-value[b]);
        if(k!=va)
            ans++;
            //cout<<a<<" "<<b<<" "<<k<<endl;
    }
    }
    int main()
    {
    int m,n;
    //ios::sync_with_stdio(false);
    while(scanf("%d%d",&m,&n)!=EOF)
    {
        fro(i,0,m+1)
        {
            root[i]=i;
        }
        mem(value,0);
        int a,b,va;
        ans=0;
        fro(i,0,n)
        {
            scanf("%d%d%d",&a,&b,&va);
            a--;
            unionroot(a,b,va);
        }
        printf("%d\n",ans);
    }
    return 0;
    }

### HihoCoder - 1515 分数调查 带权并查集
题目链接：<https://vjudge.net/problem/HihoCoder-1515><br/>
小Hi的学校总共有N名学生，编号1-N。学校刚刚进行了一场全校的古诗文水平测验。  

学校没有公布测验的成绩，所以小Hi只能得到一些小道消息，例如X号同学的分数比Y号同学的分数高S分。  

小Hi想知道利用这些消息，能不能判断出某两位同学之间的分数高低？<br/>


直接套用带权并查集模板即可。<br/>


### A Bug's Life POJ - 2492  带权种类并查集
题目链接：<https://vjudge.net/problem/POJ-2492><br/>
这道题是作为种类并查集的基础入门题的存在，主要就是说给咱们n组关系，来判断其中有没有 gay虫。对，所谓gay虫，就是性别一样的虫，哈哈哈，这里的权值是性别，0表示同性，1表示异性（这样好除余）<br/>
 
### Find them, Catch them POJ - 1703 与上一题gay虫差不多
题目链接：<https://vjudge.net/problem/POJ-1703><br/>
并查集把给出的人分成几个集合,每个集合之间的人的关系不确定，对同一个集合,保存和本人不为同一队的人,本着敌人的敌人便是朋友的原则,用并查集同一集合为同一队,不同集合为不同队。<br/>


### 带权并查集之在升级---洛谷 P1196银河英雄传说
题目链接：<https://www.luogu.org/problem/P1196><br/>
带偏移量的并查集，比普通的并查集多了两个数组 ，value[i]记录i点以前的数量（不包含i点），num[i]记录i点以后的数量（包含i点），然后每次查询祖先的时候顺便更新fro值，很神奇的思路啊~ <br/>

### 青蛙约会更正
前几天的青蛙约会中，可能由于vj数据太水，以至于我在洛谷上都不能全A，事后了解，exgcd不能系数不能为负。<br/>


### 图论  最短路径 Floyd-Warshall算法 A - 最短路 
题目链接：<https://vjudge.net/contest/320737#problem/A><br/>
典型图论最短路问题，有多种方法，这里我只写了上述算法。<br/>

### 拓扑排序模板 之优先队列使用 K - 确定比赛名次 
题目链接：<https://vjudge.net/contest/320737#problem/K><br/>
拓扑排序模板，需要注意的是要使用优先队列，因为题目说了最小字典序输出。<br/>

### 数学hunter hdu 4438
题目链接:<http://acm.hdu.edu.cn/showproblem.php?pid=4438><br/>
数学公式题，需要更据题意退出公式，注意两个猎人杀完一个要去杀第二个。<br/>







