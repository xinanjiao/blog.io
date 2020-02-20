---
layout: post
title: poj 1035(暴力) uva 11584(动态规划)
date: 2019-4-22
categories: blog
tags: [动态规划]
description: 语言
---

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













