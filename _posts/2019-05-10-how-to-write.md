---
layout: post
title: STL
date: 2019-5-11
categories: blog
tags: [算法]
description: 语言
---

### unique()函数
unique:从每个相等元素的连续组中去除(不是删除，只是和其它不相同的元素交换了位置)除第一个以外所有的元素。

        换句话说，unique函数的作用是去除相邻重复元素,因此在使用unique函数时,必须确保所有重复值一个接着一个（既必须先排好序）。

        unique_copy与unique的唯一区别在于：unique_copy会将进行删除相邻重复元素的结果 保存在另外一个结果容器中。函数参数：unique_copy(first,last,result,compare); first为容器的首迭代器，last为容器的末迭代器，result为保存结果的容器（原容器的内容不变），compare为比较函数（可略写）。

现在来一个例子：

    #include<bits/stdc++.h>
    using namespace std;
    int main()
    {
    int a[10]={1,6,3,5,3,5,5,9,10,1};
    int i;
    sort(a,a+10);//注意去重前一定要排序
    int m=unique(a,a+10)-a;//unique返回的是尾指针，剪掉a则为元素个数
    for(i=0;i<m;i++)//输出
        cout<<a[i]<<" ";
    }

输出结果:1 3 5 6 9 10

### set()内置函数
set来就是集合，所以肯定带有集合的特性，比如求交集，并集，差集(A-B);<br/>

set里面有set_intersection（取集合交集）、set_union（取集合并集）、set_difference（取集合差集）、set_symmetric_difference（取集合对称差集）等函数。其中，关于函数的五个参数问题做一下小结：

1、这几个函数的前四个参数一样，只有第五个参数有多重版本。

2、EX1：set_union(A.begin(),A.end(),B.begin(),B.end(),inserter( C1 , C1.begin() ) );前四个参数依次是第一的集合的头尾，第二个集合的头尾。第五个参数的意思是将集合A、B取合集后的结果存入集合C中。

EX2：set_union(A.begin(),A.end(),B.begin(),B.end(),ostream_iterator<int>(cout," “));这里的第五个参数的意思是将A、B取合集后的结果直接输出，（cout," "）双引号里面是输出你想用来间隔集合元素的符号或是空格。

<span style="font-family:Comic Sans MS;font-size:18px;">
/*Description
集合的运算就是用给定的集合去指定新的集合。设A和B是集合，则它们的并差交补集分别定义如下：
A∪B={x|x∈A∨x∈B}
A∩B={x|x∈A∧x∈B}
A-B={x|x∈A∧x不属于 B}
SA ={x|x∈(A∪B)∧x 不属于A}
SB ={x|x∈(A∪B)∧x 不属于B}
<!--[endif]-->
Input<br/>
第一行输入一个正整数T，表示总共有T组测试数据。（T<=200）<br/>
然后下面有2T行，每一行都有n+1个数字，其中第一个数字是n(0<=n<=100)，表示该行后面还有n个数字输入。<br/>
Output<br/>
对于每组测试数据，首先输出测试数据序号，”Case #.NO”，<br/>
接下来输出共7行，每行都是一个集合，<br/>
前2行分别输出集合A、B，接下5行来分别输出集合A、B的并(A u B)、交(A n B)、差(A – B)、补。<br/>
集合中的元素用“{}”扩起来，且元素之间用“， ”隔开。<br/>
Sample Input<br/>
1<br/>
4 1 2 3 1<br/>
0<br/>
Sample Output<br/>
Case# 1:<br/>
A = {1, 2, 3}<br/>
B = {}<br/>
A u B = {1, 2, 3}<br/>
A n B = {}<br/>
A - B = {1, 2, 3}<br/>
SA = {}<br/>
SB = {1, 2, 3}<br/>
*/

    #include<bits/stdc++.h>
    using namespace std;
    int main()
    {
    set<int>A;
    set<int>B;
    set<int>C1;
    set<int>C2;
    set<int>C3;
    set<int>C4;
    set<int>C5;
    set<int>C6;
    set<int>::iterator pos;/// 定义迭代器，作用是输出set元素
    int count=0;
    int A_i,B_i,n,m;
    cin>>n;
    while(n--)
    {
        count++;
        cin>>A_i;
        while(A_i--)///输入集合A
        {
            cin>>m;
            A.insert(m);
        }
        cin>>B_i;///输入集合B
        while(B_i--)
        {
            cin>>m;
            B.insert(m);
        }
        cout<<"Case# "<<count<<":"<<endl;
 
        cout<<"A = {";
        for(pos=A.begin(); pos!=A.end(); pos++)///迭代器的作用
        {
            if(pos!=A.begin())cout<<", ";
            cout<<*pos;///迭代器的作用，迭代器是一种特殊的指针
        }
        cout<<"}"<<endl;
 
        cout<<"B = {";
        for(pos=B.begin(); pos!=B.end(); pos++)
        {
            if(pos!=B.begin())cout<<", ";
            cout<<*pos;
        }
        cout<<"}"<<endl;
 
        set_union(A.begin(),A.end(),B.begin(),B.end(),inserter( C1 , C1.begin() ) );    /*取并集运算*/
        //set_union(A.begin(),A.end(),B.begin(),B.end(),ostream_iterator<int>(cout," "));    /*取并集运算*/ //其中ostream_iterator的头文件是iterator
        cout<<"A u B = {";
        for(pos=C1.begin(); pos!=C1.end(); pos++)
        {
            if(pos!=C1.begin())cout<<", ";
            cout<<*pos;
        }
        cout<<"}"<<endl;
 
        set_intersection(A.begin(),A.end(),B.begin(),B.end(),inserter( C2 , C2.begin() ));    /*取交集运算*/
        cout<<"A n B = {";
        for(pos=C2.begin(); pos!=C2.end(); pos++)
        {
            if(pos!=C2.begin())cout<<", ";
            cout<<*pos;
        }
        cout<<"}"<<endl;
 
        set_difference( A.begin(), A.end(),B.begin(), B.end(),inserter( C3, C3.begin() ) );    /*取差集运算*/
        cout<<"A - B = {";
        for(pos=C3.begin(); pos!=C3.end(); pos++)
        {
            if(pos!=C3.begin())cout<<", ";
            cout<<*pos;
        }
        cout<<"}"<<endl;
 
        set_difference(C1.begin(),C1.end(), A.begin(), A.end(),inserter( C4, C4.begin() ) );/*取差集运算*/
        cout<<"SA = {";
        for(pos=C4.begin(); pos!=C4.end(); pos++)
        {
            if(pos!=C4.begin())cout<<", ";
            cout<<*pos;
        }
        cout<<"}"<<endl;
 
        set_difference(C1.begin(),C1.end(), B.begin(), B.end(),inserter( C5, C5.begin() ) );/*取差集运算*/
        cout<<"SB = {";
        for(pos=C5.begin(); pos!=C5.end(); pos++)
        {
            if(pos!=C5.begin())cout<<", ";
            cout<<*pos;
        }
        cout<<"}"<<endl;
 
        set_symmetric_difference(A.begin(),A.end(),B.begin(),B.end(),inserter( C6 , C6.begin() ) );///取 对称差集运算
        cout<<"A ⊕ B = {";
        for(pos=C6.begin(); pos!=C6.end(); pos++)
        {
            if(pos!=C6.begin())cout<<", ";
            cout<<*pos;
        }
        cout<<"}"<<endl;
 
        A.clear();
        B.clear();//各个集合清零，否则下次使用会出错
        C1.clear();
        C2.clear();
        C3.clear();
        C4.clear();
        C5.clear();
        C6.clear();
    }
}
</span>









