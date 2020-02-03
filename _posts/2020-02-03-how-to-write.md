---
layout: post
title: codeforce round 616 div2
date: 2020-2-03
categories: blog
tags: [codeforce]
description: 语言
---

### Today
今天心情很浮躁，在网上看了很多新闻。官方的，非官方的。我一直是个很喜欢国家的人。但在网上看到了这场疫情爆发之后，中国政府的总总不作为，低效率，就感觉很难受。<br>

我就很矛盾，今天脑子里两个小人打了好久的架了，都不想做题，好浮躁啊！按理来说跟我没啥关系啊，但看了微博武汉求助者的超话之后，就觉得很难受，像自己经历的一样。网上一句话让我印象深刻：“在家中因为得不了救助而死去的人们，连成为新型冠状病毒死亡人数后的冰冷的数字都不行。”<br>

21世纪的现在，我却看到了，几十年前的状况。<br>

悲矣，我也只能写写我的感受，却无所作为。我就要让以后的自己记住：不管以后怎样，也要有十足的打算，在天灾面前，人类还是很渺小。

## codeforce div2
### A - Even But Not Even CodeForces - 1291A 
#### 题目大意
给出一个数。你可以删除任何一位数字，使它满足每个数字之和能被2整除，但数字本身不能被2整除。

#### 思路
贪心吧最后一位找到奇数之后，再在前面找一个奇数就行了。

	int main(){
    ios::sync_with_stdio(0);
    int t;
    cin>>t;
    while(t--){
        mem(book,0);
        int n,a,sum=0;
        cin>>n;
        string s;
        cin>>s;
        fro(i,0,n)
        sum+=(s[i]-'0');
        a=s[n-1]-'0';
        while(a%2==0){
            sum-=a;
            s.erase(n-1,1);
            n--;
            if(n==0)
                break;
            a=s[n-1]-'0';
        }
        if(sum%2==0&&s.size()!=0)
            cout<<s<<endl;
        else if(s.size()==0)
            cout<<-1<<endl;
        else{
            string ss="";
            int sum=0,pos1=0,pos2=0,ok=0;
            fro(i,0,n-1){
                int a=s[i]-'0';
        //cout<<"??"<<a<<" "<<endl;
                if(a%2!=0){
                    ss.push_back(s[i]);
                    ok=1;
                    break;
                }
            }
            ss.push_back(s[n-1]);
            if(!ok)
                cout<<-1<<endl;
                else
                    cout<<ss<<endl;
        }
    }
    return 0;
	}

### B - Array Sharpening CodeForces - 1291B 
#### 题目大意
给一个长度为n的数组，问能否通过操作把该数组变成先严格递增，然后再严格递减的数组
操作为如果一个数大于0，可以把他-1，对于每个数可以进行任意次

#### 思路
贪心，为了更好的让他满足题意，我们先考虑递增的，只要a[i]>=i-1，是不是他就可以满足变成i-1，数组变成这样012345…，当遇到不能变成i-1的时候，只能开始递减，为了更好的满足，每次我们只让他比前一个数小1，当递减到0时，整个数组还没有满足，说明再也不可能满足，输出No，否则输出Yes


	int a[maxn];
	int main()
	{
    int t,n;
    scanf("%d",&t);
    while(t--){
        scanf("%d ",&n);
        for(int i=1;i<=n;i++)scanf("%d",&a[i]);
        int flag=1;
        int p,pos=n+1;
        for(int i=1;i<=n;i++){//递增
            if(a[i]<i-1){
                pos=i;
                p=a[i-1];
                break;
            }
        }
        //cout<<pos<<endl;
        //cout<<pos<<endl;
        for(int i=pos;i<=n;i++){//递减
            if(a[i]<p)p=a[i];
            else{
                if(p-1>=0)p=p-1;
                else {
                    flag=0;
                    break;
                }
            }
        }
        if(flag)printf("Yes\n");
        else printf("No\n");
    }
    return 0;
	}


## 武汉加油！！！！！！！




