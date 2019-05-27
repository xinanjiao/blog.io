---
layout: post
title: STL补充
date: 2019-5-20
categories: blog
tags: [算法,语言]
description: 语言
---
唉，掐指一算，我已经好久没有写博客了，但我也在一直学习，这学期也接近尾声了，各种考试接踵而至，时间也变得越来越紧迫。总结一下最近学的东西。

## map
map是c++STL里面的关联式容器，map<键，值>,把键和值一一对应起来，用起来很方便，他可以把和复杂的键转化为非常简单的值，它是有序无重复的。其他操作与其他容器类似

修改

　　　　insert　　插入

　　　　　　　　　(1) pair<iterator, bool> insert(const pair<key, value> &val);

　　　　　　　　　　 // 单个值插入，参数为 pair类型，first 为  key, second为 value

　　　　　　　　　　 // 返回值也是一个 pair，first为插入后的 iterator，second 为bool类型， true表示插入成功，false 表示插入失败

　　　　　　　　　　 // 插入失败是因为 map 中已经有一个 key 与输入相同。这次插入操作对map 不会有任何影响， 失败时返回值的 first指向已有的key-value

　　　　　　　　　(2) iterator insert(const_iterator pos, const pair<key, value> &val);

　　　　　　　　　　  // 提示值插入

　　　　　　　　　　  // 从 pos 指定的位置开始查找 val应该插入的位置

　　　　　　　　　　  // 如果设定值合适，可以减少插入时做查找的时间

　　　　　　　　　(3) void insert(iterator first, iterator second);

　　　　　　　　　　  // 范围插入，插入[first, second)范围内的内容

　　　　　　　　　(4) void insert(initializer_list<value_type> il);

　　　　　　　　　　  // 初始化列表插入

     make_pair()  插入
   
     和insert不同，make_pair(k,v)把k,v作为一个不可分割的整体插入，make_pair将用<k,v>构造映射元素。

     
     元素访问

     map映射元素有<键，值>对构成，且同一个键可以对应多个值，可以通过相关迭代器访问他们的元素，map类型迭代器提供了两个数据成员：一个是first,另一个是second,用于访问值。
    

    #include <map>
    #include <string>
    #include <iostream>
    using namespace std;
    int main()
    {
    map<int, string> mapStudent;
    mapStudent.insert(pair<int, string>(1, "student_one"));
    mapStudent.insert(pair<int, string>(2, "student_two"));
    mapStudent.insert(pair<int, string>(3, "student_three"));
    map<int, string>::reverse_iterator  iter;
    for(iter = mapStudent.rbegin(); iter != mapStudent.rend(); iter++)
    {
        cout<<iter->first<<" "<<iter->second<<endl;
    }
    system("pause");
    return 0;
    }

    map类型的键可用作数组下标，访问该键的值。

    #include <map>
    #include <string>
    #include <iostream>
    using namespace std;
    int main()
    {
    map<int, string> mapStudent;
    mapStudent.insert(pair<int, string>(1, "student_one"));
    mapStudent.insert(pair<int, string>(2, "student_two"));
    mapStudent.insert(pair<int, string>(3, "student_three"));
    int nSize = mapStudent.size();
    for(int nIndex = 1; nIndex <= nSize; nIndex++) 
    {
        cout<<mapStudent[nIndex]<<endl;
    }
    system("pause");
    return 0;
    }
            












