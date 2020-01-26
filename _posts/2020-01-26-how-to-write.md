---
layout: post
title: codeforce div3 615
date: 2020-1-26
categories: blog
tags: [codeforce]
description: 语言
---

### 写在前面
今年的过年印象挺让人深刻的，过年都过得人心惶惶。现在各种饭局都延迟不办了。非典那时还小，如今虽然这个冠状病毒现在还不是那么厉害，但传染力比非典强很多，短短几天破千，当然，这与春运是分不开的。希望奋斗在防疫工作前线的医务人员都要好好的，武汉加油！中国加油！

### Collecting Coins CodeForces - 1294A 
#### 题目大意
给出 a,b,c,n，判断是否存在 A,B,C 满足 (A+B+C=n 且 a+A=b+B=c+C) 。

多组数据，1≤t≤104，1≤a,b,c,n≤108 且 (a,b,c,n) 均为正整数，(A,B,C) 均为非负整数。

#### 思路
判断a+b+c+n是否被三整除，不能就NO，能就判断平均数是否小于a,b,c任意一个数，成立就NO，否则YES

    int main(){
    ios::sync_with_stdio(0);
    int t;
    cin>>t;
    while(t--){
        int a,b,c,n;
        cin>>a>>b>>c>>n;
        ll sum=0;
        sum=a+b+c+n;
        if(sum%3!=0)
            cout<<"NO"<<endl;
        else{
            int ss=sum/3;
            if(ss<a||ss<b||ss<c)
                cout<<"NO"<<endl;
            else
                cout<<"YES"<<endl;
        }
    }
    return 0;
    }

### Collecting Packages CodeForces - 1294B
#### 题目大意
给出 nn 与 nn 个特殊点的坐标 (xi,yi)(xi,yi) 。

 从 (0,0) 出发， 只能向上'U'和向右'R'移动，求最优路径，或告知无解。

多组数据，1≤t≤100，1≤n≤1000，0≤xi,yi≤1000。

#### 思路
刚开始一直mle,好吧，自己的问题，代码有bug。<br>
只能向左和向前，判断不能成立的条件就是不能后退和向右
<p style="color: red">不同的编译器不一样，我以前的代码，c++11.5编译器是返回mle，而vs2017是返回tle，所以要具体分析</p>

    struct node{
    int x,y;
    node(){}
    };
    bool cmp(node a,node b){
    if(a.x==b.x)
        return a.y<b.y;
    else
        return a.x<b.x;
    }
    int main(){
    ios::sync_with_stdio(0);
    int t;
    cin>>t;
    while(t--){
        node a[1001];
        int n;
        cin>>n;
        fro(i,0,n){
            cin>>a[i].x>>a[i].y;
        }
        sort(a,a+n,cmp);
        bool ok=0;
        int x=a[0].x,y=a[0].y;
        fro(i,1,n){
            if(a[i].x==x){
                if(a[i].y<y){ok=1;break;}
                else{
                    y=a[i].y;
                }
            }
            else{
                if(a[i].x>x&&a[i].y>=y){
                        x=a[i].x;
                        y=a[i].y;
                }
                else{ok=1;break;}
            }
        }
        if(ok==1)
            cout<<"NO"<<endl;
        else{
            string s="";
            int aa=0,b=0;
            fro(i,0,n){
                int x=a[i].x,y=a[i].y;
                int cnt1=x-aa,cnt2=y-b;
                while(cnt1--)
                    s+="R";
                while(cnt2--)
                    s+="U";
                aa=x,b=y;
            }
            cout<<"YES"<<endl;
            cout<<s<<endl;
        }
    }
    return 0;
    }

### Product of Three Numbers CodeForces - 1294C 贪心

#### 题目大意
给出一个 n，求 x,y,z 满足 (x⋅y⋅z=n 且 x,y,z≥2 且 x≠y≠z)，或者告知无解。

多组数据，1≤t≤100，2≤n≤1e9。

给出 n 个特殊点的坐标 (xi,yi) 。

从 (0,0)出发， 只能向上'U'和向右'R'移动，问是否可以经过每个特殊点，可以则输出路径。


#### 思路
贪心选择最小的因数，保证后面的数最大，暴力查找即可

    int main(){
    ios::sync_with_stdio(0);
    int t;
    cin>>t;
    while(t--){
        int n;
        cin>>n;
        map<int,int> mp;
        int a=0,b=0,c;
        for(int i=2;i<=maxn;i++){
            if(n%i==0){
                mp[i]=1;
                a=i;
                n/=i;
                break;
            }
        }
        for(int i=2;i<=maxn;i++){
            if(n%i==0&&!mp[i]){
                b=i;
                n/=i;
                break;
            }
        }
        c=n;
        if((a==c||b==c||a==b)||(a<2||b<2||c<2)){
            cout<<"NO"<<endl;
        }
        else{
            cout<<"YES"<<endl;
            cout<<a<<" "<<b<<" "<<c<<endl;
        }
    }
    return 0;
    }


### MEX maximizing CodeForces - 1294D 贪心

#### 题目大意
 定义数组中的 MEX 表示不属于该数组的最小非负整数，example:[0,0,1,0,2]的 MEX 就是 3。

给出 q,x，您拥有一个空数组 a[](即数组中没有元素)。

您还得到 q次询问，第 jj 个询问由一个整数 yj 组成，意味着数组将多一个元素yj，查询后长度增加1。

你可以选择任意元素 ai 并让 ai+=x 或 ai−=x，且可以操作任意次。

 您需要进行以上操作，并且最大化 a 数组的 MEX，并输出。

#### 思路
贪心选最小，这里很巧妙，最后的数不会大于q，看余数选择即可，尽量最小

    vector<int> s[maxn];
    int main(){
    //ios::sync_with_stdio(0);
    int q,x;
    cin>>q>>x;
    int mex=q;
    map<int,int> mp;
    int ans=0,cnt=0;
    while(q--){
        int m;
        cin>>m;
        int a=m%x;
        int len=s[a].size();
        if(len==0){
            s[a].push_back(a);
            mp[a]=1;
        }
        else{
            int ss=s[a][len-1];
            ss+=x;
            if(ss<=mex){
            s[a].push_back(ss);
            mp[ss]=1;
            }
        }
        while(mp[ans]&&ans<=mex){
            ans++;
        }
        cout<<ans<<endl;
    }
    return 0;
    }