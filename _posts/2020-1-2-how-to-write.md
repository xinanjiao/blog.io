---
layout: post
title: 二分交互+hash模板
date: 2020-1-2
categories: blog
tags: [交互题,中途相遇法]
description: 文章金句。
---

###  Guess the Array CodeForces - 727C 思维（交互）

#### 题目大意
隐藏一个数组a，你根据你的提问所返回的结果，得出数组元素。方式如下：你可以询问任意两个元素的和，最多问n次，n为数组的长度。

#### 思路：
一道数学题，思路很简单，你可以问三次，就可以得到前三个元素的值，然后依据其中一个已知元素的值，得出其他n-3个元素的值。

    int main()
    {
    ios::sync_with_stdio(0);
    int n;
    cin>>n;
    int sum=0;
    vector<int> s;
    int a[3],ans=0;
    for(int i=1;i<=n;i++)
    {
        sum++;
        if(sum==1)
        {
            cout<<"? 1 2"<<endl;
            cin>>a[0];
        }
        else if(sum==2)
        {
            cout<<"? 1 3"<<endl;
            cin>>a[1];
        }
        else if(sum==3)
        {
            cout<<"? 2 3"<<endl;
            cin>>a[2];
            ans=(a[0]+a[1]-a[2])/2;
            s.push_back(ans);
            s.push_back(a[0]-ans);
            s.push_back(a[1]-ans);
        }
        else
        {
            cout<<"? 1 "<<i<<endl;
            fflush(stdout);
            int x;
            cin>>x;
            s.push_back(x-ans);
        }
    }
    cout<<"!";
    fro(i,0,s.size())
    {
        cout<<" "<<s[i];
    }
    return 0;
    }


### Mahmoud and Ehab and the binary string CodeForces - 862D 二分交互
#### 题目大意
隐藏一串最长为1000长度的01二进制字符串，你通过询问相同长度的01字符串，然后返回你汉明距离（自行百度）。通过所返回的结果，得出串中任意两个0，1的位置，询问次数不大于15次。

#### 思路
样例看起来是枚举，但这不可能，1000的长度，15次询问。这让我们想到了二分，2的10次方1024，很接近。确定方向后，想想细节。我们最终想得到的是0和1的位置，因为题目保证必定有1和0，所以必定存在01或10字符串，我们的任务就是二分出这个字符串。二分的方法很巧妙。<br>
1.输入全为0的字符串，得出1的个数<br>
2.开始二分，二分过程中，判断左边全为1或者全为0，见代码。通过这个条件判断怎么二分。<br>
3.确定01还是10子序列<br>


    int main()//二分字符串，求01或10字符串
    {
    ios::sync_with_stdio(0);
    int n;
    cin>>n;
    cout<<"? ";
    fro(i,0,n)
    cout<<0;
    cout<<endl;//获取1的个数
    int x;
    cin>>x;
    int num=x;//1的个数
    int l=1,r=n;//开始二分子串
    while(r-l>=2)
    {
        int mid=l+(r-l)/2;
        cout<<"? ";
        for(int i=1;i<=n;i++)
        {
            if(i>=l&&i<=mid)//左区间
                cout<<1;
            else
                cout<<0;
        }
        cout<<endl;
        cin>>x;
        if(abs(num-x)==(mid-l+1))//若左区间全为0或为1
         l=mid;
         else
            r=mid;
    }
    //已经找到01或10串了，现在判断0在前还是在后
    cout<<"? ";
    for(int i=1;i<=n;i++)
    {
        if(i==l)
            cout<<1;
        else
            cout<<0;
    }
    cout<<endl;
    cin>>x;
    cout<<"! ";
    if(x<num)
        cout<<r<<" "<<l<<endl;
        else
            cout<<l<<" "<<r<<endl;
    return 0;
    }


### 4 Values whose Sum is 0 UVA - 1152  中途相遇法/手动hash
#### 题目大意





