---
layout: post
title: UVA象棋
date: 2019-5-6
categories: blog
tags: [算法,模拟,uva]
description: 语言
---
![象棋](/img/uva1589.png)
### 题目大意
输入一个残局，棋盘为10x9，其中黑方只有一个将，红方由你输入2-7个棋子，但是其中必须有帅（G），还可以有车（R），马（H），炮（C），其中具体规则和中国象棋一样，但有个撇马脚，具体看上图，还有双方将帅对面，称为”飞帅“。判断将走一步后，是否被红方将军，为死局，是输出YES，不是输出NO。<br/>

自己的思路：最先想了想，将只能走一步，最初不是有个棋盘吗，输入红方棋子，判断各个棋子所能攻击的区域，如果将走进这个区域，就死辽，将的区域限制在一个田字格里，枚举将能走的几个方向，分别判断是否被红方棋子消灭，如果所有方向均被消灭，则输出NO，但是撇马脚的情况很难判断。而且如果将把它旁边的一个棋子吃了，那么被吃的红方棋子标记的消灭区域都被消除。。。。。总之撇马脚是个难点，在网上浏览了很久，发现一种很巧妙的方法，而且代码量还不大。<br/>

    #include<bits/stdc++.h>
    using namespace std;
    char qipan[12][12];
    int hr[4]={1,-1,0,0},hc[4]={0,0,-1,1};
    int chr[4]={0,0,1,-1},chc[4]={-1,1,0,0};
    int hhr[8]={-2,-1,-2,-1,1,2,1,2};
    int hhc[8]={-1,-2,1,2,-2,-1,2,1};//假想将像马一样行走
    int hlr[8]={-1,-1,-1,-1,1,1,1,1};
    int hlc[8]={-1,-1,1,1,-1,-1,1,1};//撇马脚***
    int check(int a,int b)
    {
      if (a < 1 || a > 3 || b < 4 || b > 6) return 1;
    for(int i=0;i<4;i++)//碰见车炮帅
    {
        int cnt=0;
        int cnr=a+chr[i],cnc=b+chc[i];
        for(;cnr>0&&cnr<=10&&cnc>0&&cnc<=9;cnr+=chr[i],cnc+=chc[i])//纵横四方
        {
            if(qipan[cnr][cnc])//有东西？
            {
                if((qipan[cnr][cnc]=='R'||qipan[cnr][cnc]=='G')&&cnt==0)
                    return 1;//死辽
                else if(qipan[cnr][cnc]=='C'&&cnt==1)//炮前有山
                return 1;//死辽
                 cnt++;
            }
        }
    }
    for(int i=0;i<8;i++)
    {
        int cnr=a+hhr[i],cnc=b+hhc[i];
        if(cnr>0&&cnr<=10&&cnc>0&&cnc<=9&&qipan[cnr][cnc]=='H'&&!qipan[a+hlr[i]][b+hlc[i]])
            return 1;
    }
    return 0;
    }
    int main()
    {
    int sum,heir,heic,honzir,honzic;
    char s;
    while(cin>>sum>>heir>>heic&&sum&&heir&&heic)
    {
        int con=0;
        memset(qipan,0,sizeof(qipan));
        for(int i=0;i<sum;i++)
        {
            cin>>s>>honzir>>honzic;
            qipan[honzir][honzic]=s;
        }
        for(int i=0;i<4;i++)
        {
            con+=check(heir+hr[i],heic+hc[i]);
        }
        if(con>=4)
                cout<<"YES"<<endl;
            else
                cout<<"NO"<<endl;
     }
    }

### 代码思想
这个思想和我上述的思想就完全不一样了。我的思路是一开始就把死亡的地方标记，进去就死辽。他是把将移动一个位置后，就把它四面八方所有的位置枚举出来，看看有没有红方棋子。<br/>
黑方的将不是要四个方向走吗？行！就创建了Hr[4]和Hc[4]来进行将的移动。<br/>

红方车，帅，车帅看见将就把他带走，前提是他们前面没有东西，但是炮有一个特性，炮相比其他两个比较特殊，因为它前面必须有个棋子才能把将带走，所以当将走到下一个坐标是，枚举它的四面八方，如果遇到车帅，GG，用个计数器，如果计数器为1，表示炮前有东西，将GG。如何四面八方走，这是又有两个模拟数组chr[4],chc[4]。<br/>

行了，到了重难点马的判断。我们知道如果马能将军，说明将如果像马这样走的话也可以走到马的位置，所以就有将模拟马走，
像马一样走，枚举出所有点，这时就有了hhr[8]，和hhc[8]这两个数组，如果将走到一个位置有马，所以马就可以将军，这时我们就要判断撇马脚，撇马脚有很多种情况，如果全部一起分析，很容易混乱，所以把每一种情况的撇马脚枚举出来，所以就有hlr[8],hlc[8]这两个数组。用这两个数组判断马是否撇马脚，就是是否有棋子撇它。<br/>

整体就是这样。对！很难，但ACM全名就是also challenge miracle,可不能轻易说放弃，要向AC的道路进军啊<br/>
## 加油！









