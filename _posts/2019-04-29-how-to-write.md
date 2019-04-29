---
layout: post
title: BF（暴风算法）
date: 2019-4-29
categories: blog
tags: [算法,BF]
description: 语言
---
## Bf算法
BF算法，即暴风(Brute Force)算法，是普通的模式匹配算法，BF算法的思想就是将目标串S的第一个字符与模式串T的第一个字
符进行匹配，若相等，则继续比较S的第二个字符和 T的第二个字符;若不相等，则比较S的第二个字符和T的第一个字符，依次比较下去，直到得出最后的匹配结果。BF算法是一种蛮力算法。<br/>

  此算法在于不断的比较，比较失败，回溯，重新在上一个位置后面一个位置加以比较~~看图更明白~~：

举例：
>S:ababcababa<br/>
>T: ababa

  ![BF算法](/img/4293.png)
  ![BF算法](/img/4292.png)
  **该算法最糟糕要进行M*(N-M+1)次比较，时间复杂度为O(M*N)。<br/>

暴风算法~~名字太酷辽~~，在许多问题上运用很多，比如uva上1588<br/>

### 大致题意：
给你连个长度分别为n1，n2且每列高度只为1或2的长条，然后将他们拼在一起，高度不能超过3，问他们拼在一起的最短长度。
![img](/img/4291.png)
![img](/img/429.png)


>sample input
>>2112112112

>>2212112

>>12121212

>>21212121

>>2211221122

>>21212

>sample output
>>10
>>8
>>15


**思路分析**：你不认真想想，你还真不太知道题意~~我是这样的，可能太菜了~~，实际上想通了过后就很简单了，容器高3，你就比较两个字符串，里面的字符相加小于等于3。<br/>
那你的任务肯定是逐个比较两个字符串中相应位置的字符的和，我觉得肯定还有其他算法更好，但无法遮掩暴风大法的独特魅力！这道题要在原来的模板上稍作修改。

    #include <bits/stdc++.h>//BF算法
    using namespace std;
    int BF(string &s1,string &s2)//BF算法内部
    {
    int a=s1.size(),b=s2.size();比较长度
    int i=0,j=0;
    while(i<a&&j<b)
    {
        if((s1[i]-'0')+(s2[j]-'0')<=3)
        {
            i++;
            j++;
        }//满足条件，继续比较
        else
        {
            i=i-j+1;
            j=0;//不满足，回溯，重新比较
        }
    }
    if(j>=b||i>=a)
        return i-j;//返回最先开始满足条件的位置
    }
    int main()
    {
    string a,b,temp;
    int wz,len,zw,len2;
    while(cin>>a>>b)
    {
        if(a.size()>=b.size())
        {
            temp=a;
            a=b;
            b=temp;
        }
            wz=BF(a,b);
        if(a.size()==(wz+1)||b.size()==(wz+1)&&(wz+1)!=1)
            len=a.size()+b.size();
        else if(a.size()==1&&b.size()==1&&(wz+1)==1)
            len=1;
        else
        {
            len=b.size()+wz;
        }
        zw=BF(b,a);
        if((a.size()==(zw+1)||b.size()==(zw+1))&&zw!=0)
            len2=a.size()+b.size();
        else if(a.size()==1&&b.size()==1&&zw==0)
            len2=1;
            else
            {
                if((b.size()-zw)>=a.size())
                    len2=b.size();
                else
                    len2=a.size()+zw;
            }
            cout<<min(len,len2)<<endl;
    }
    }

BF算法模板：


    int BF(string &s,string &p,pos) 
	{
	int lens = strlen(s);
	int lenp = strlen(p);
	if (lens < lenp)      //如果S串的长度都小于P串了，那么肯定不存在P串能匹配上S串
	{
		return -1;
	}
	int i = pos, j = 0;
	while (i < lens && j < lenp)
	{
		if (s[i] == p[j])
		{
			i++;
			j++;
		}
		else
		{
			i = i - j + 1;  // 使i回退到本次比较的初始位置的下一个位置
			j = 0;
		}
	}
	if (j >= lenp) // 如果j越界，证明比较时已经将p串访问完，则找到了相等的串
	{
		return i - j;  //返回已完成匹配之后S串中开始匹配的位置
	}
	return -1;
    }









