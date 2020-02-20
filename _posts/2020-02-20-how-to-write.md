---
layout: post
title: poj 1035(暴力) uva 11584(动态规划)
date: 2020-02-20
categories: blog
tags: [动态规划]
description: 语言
---

今天在看poj的km二分图最大流的时候，先花半天的时间复习了KM算法。。。最后居然那两道题不是考km算法，居然是网络流的EK算法，我就被劝退了，就先暂时跳过吧。


### Spell checker POJ - 1035 
#### 题目大意
给出一个字典，存放字符串。现在输入待查询的字符串，判断该字符串经过如下操作：
1. 任意替换一个字符
2. 任意删除一个字符
3. 任意添加一个字符
后是否在字典中，在输出字典中的字符串，如果原串就存在则输出：xxx is correct。

#### 思路
暴力即可。在字典中判断与待判断的串长度之差的绝对值小于等于一的串。再暴力检查是否满足以上条件。<br>
值得注意的是，我交G++一直超时，然后c++就179ms，不知道是什么操作！！

```
string s1[maxn];
string s2[maxn];
int main()
{
    ios::sync_with_stdio(false);
    string s;
    int cnt1=0,cnt2=0;
    while(cin>>s&&s!="#"){
        //cin>>s;
        s1[cnt1++]=s;
       // cout<<s<<endl;
    }
    while(cin>>s&&s!="#"){
       // cin>>s;
        s2[cnt2++]=s;
        //cout<<s<<endl;
    }
    for(int i=0;i<cnt2;i++){
        bool flag=0;
        s=s2[i];
        //cout<<s<<endl;
        for(int j=0;j<cnt1;j++){
            if(s1[j]==s){
                flag=1;
                break;
            }
        }
        if(flag){
            cout<<s<<" is correct"<<endl;
            continue;
        }
        else{
            cout<<s<<":";
            int len1=s.size();
            for(int j=0;j<cnt1;j++){
                int len2=s1[j].size();
                if(len1==len2){//替换
                    int sum=0;
                    for(int i=len2-1;i>=0;i--){
                        if(sum>=2)
                            break;
                        if(s1[j][i]!=s[i])
                        sum++;
                    }
                    if(sum==1)
                        cout<<" "<<s1[j];
                }
                else if(abs(len1-len2)==1){
                    if(len1-len2==1){//删除
                        int ans=0,sum=0;
                        for(int i=0;i<len1;i++){
                            if(sum>=2)
                                break;
                            if(s[i]!=s1[j][ans]){
                                    sum++;
                                continue;
                            }
                            else{
                                ans++;
                            }
                        }
                        if(sum==1)
                            cout<<" "<<s1[j];
                    }
                    else{//添加
                        int ans=0,sum=0;
                        for(int i=0;i<len2;i++){
                            if(sum>=2)
                            break;
                            if(s1[j][i]!=s[ans]){
                                sum++;
                                continue;
                            }
                            else 
                                ans++;
                            
                        }
                        if(sum==1)
                            cout<<" "<<s1[j];

                    }
                }
            }
            cout<<endl;
        }
    }
    return 0;
}
```



### Partitioning by Palindromes UVA - 11584   动态规划

#### 题目大意
给出一个字符串，对他进行分割得到最小的回文串，一个字符也是一个回文串。

#### 思路
**列出状态转移方程为dp[i]=min(dp[i],dp[j-1]+1(当位置i和位置j之间是回文串时))。**
决策n，总共状态转移为n * n的时间，如果边转移边判断是否为回文串就会为n^3的复杂度。<br>
所以可以先预处理回文部分，然后O(1)的判断时间。总时间为n^2。

**在c++14中，居然dp[-1]这样下标越界都不会报错，而且等于0，长知识了！！**

```
int dp[maxn];
bool ispd[maxn][maxn];
int main()
{
    ios::sync_with_stdio(false);
    int t;
    cin>>t;
    while(t--){
        mem(dp,INF);
        mem(ispd,0);
        string s;
        cin>>s;
        int n=s.size();
        for(int i=0;i<n;i++){
            int l=i,r=i;//奇数
            while(l>=0&&r<n&&s[l]==s[r]){
                ispd[l][r]=1;
                l--;r++;
            }
            l=i,r=i+1;//偶数
            while(l>=0&&r<n&&s[l]==s[r]){
                ispd[l][r]=1;
                l--;r++;
            }
        }
        //dp[0]=1;
        //dp[1]=1;
        //cout<<dp[-1]<<endl;
        for(int i=0;i<n;i++){
            for(int j=i;j>=0;j--)
                if(ispd[j][i]){
                //cout<<"j= "<<j<<" "<<dp[j-1]<<endl;
                dp[i]=min(dp[i],dp[j-1]+1);
                //cout<<i<<" "<<dp[i]<<endl;
                }
        }
        cout<<dp[n-1]<<endl;

    }
    return 0;
}
```







