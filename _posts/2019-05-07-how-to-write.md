---
layout: post
title: 字典set集合
date: 2019-5-7
categories: blog
tags: [算法,uva]
description: 语言
---
### 问题描述
输入一个文本，找出所有不同的单词（连续的字母序列），按字典序从小到大输出。单词不区分大小写。

### sample input

Adventures in Disneyland

Two blondes were going to Disneyland when they came to a fork in the
road. The sign read: "Disneyland Left."

So they went home.

### sample output
a

adventures

blondes

came

disneyland

fork

going

home

in

left

read

road

sign

so

the

they

to

two

went

were

when


## 思路
嗯，题目描述很简单，思路也很清晰，找到不同的单词按照字典序排序，这个实际上不用set也很简单，把搜集到的单词全部排序，再去重，那么怎么判断是一个单词呢，这个时候string函数来了，string是一个类型，他可以存储字符串而没有限定大小，他也是STL中的一个容器，可能你还很疑惑，我不是说的单词吗，和字符串什么关系，这个时候，stringstream来了，它是一个string类型的输入流，通过判断空格来把string类型分割为很多小字符串输入，这不就把单词确定出来了吗。

行，但是我们还是要理解今天的主角，**set集合**<br/>
什么是集合？没有重复的元素是重要的特征，它帮助我们理解了set的互异性，但是set还有排序性，它默认把集合里面的元素升序排列，如果你不想升序，你可以写一个结构体，下面再说。它和vector有很多共同的操作方式，但是它不支持push_back()操作。

定义：set<类型> a;

常用操作：<br/>
1. a.size()//返回元素个数<br/>
2. a.count(e)//返回a中e元素的个数<br/>
3. a.find(e)//返回a中e第一次出现的位置<br/>
4. a.lower_bound(e)//返回a中第一个大于等于e的元素的位置<br/>
5. a.insert(e)//当前集合中插入元素e<br/>
6. a.begin()//返回第一个元素位置，与迭代器使用<br/>

7.a.end()//返回最后一个位置，与迭代器使用<br/>


    #include<bits/stdc++.h>
    using namespace std;
    set<string> dict;
    int main()
    {
    string s,s1;
    while(cin>>s)
    {
        for(int i=0;i<s.length();i++)
            if(isalpha(s[i]))
            s[i]=tolower(s[i]);
        else
            s[i]=' ';
        stringstream ss(s);
        while(ss>>s1)
            dict.insert(s1);
    }
    for(set<string>::iterator it=dict.begin();it !=dict.end();++it)
    cout<<*it<<endl;
    }

喔，其中有个迭代器，iterator,这个记住就行了，它用来遍历容器的元素个数的.<br/>









