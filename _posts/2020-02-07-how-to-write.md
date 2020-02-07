---
layout: post
title: Codeforces Round 573 (Div2) uva 473
date: 2020-02-07
categories: blog
tags: [codeforce,动态规划]
description: 语言
---

### 开学
明天就开学了（老时间），机票也退辽。看着班级群逐渐活跃过来，发现开学的感觉也越来越浓。<br>

最近python爬虫好久没看了，队里两天一次的codeforce，确实紫书和爬虫都没啥时间搞了，得快点花时间补起来。<br>

开学了，肯定很多同学在家里猛补学习，啊啊啊，我也不能落后呀。冲冲冲。<br>

最近zf的压力很大，特别是武汉政府。唉，面对网上各种水军，不管是外国的还是国内的废青，键盘打起来就是啪啪响。唉，有错那便是有错，正义不会来缺席。真正的英雄不是在网上天天施加舆论压力的键盘侠们。而是在一线奋战的勇士们。希望我们新青年能有自己的判断能力和理性的思考准则。世间没有光，我们便做自己的炬火！

## codeforce 573 div2

### A - Tokitsukaze and Enhancement

#### 题目大意
一个数可以加0，1，2.问最后加多少输出的字母最大，准则就不说了。

#### 思路
模拟，想好再写！！

	int main(){
    ios::sync_with_stdio(0);
    int n;
    cin>>n;
    int a=n,b=n+1,c=n+2;
    vector<char> ch;
    map<char,int> mp;
    fro(i,0,3){
     int nn=n+i;
      if(nn%4==1){
            //cout<<n<<endl;
        ch.push_back('A');
        mp['A']=i;
      }
      else if(nn%4==3){
        ch.push_back('B');
        mp['B']=i;
      }
      else if(nn%4==2){
        ch.push_back('C');
        mp['C']=i;
      }
      else if(nn%4==0){
        ch.push_back('D');
        mp['D']=i;
      }
    }
    sort(ch.begin(),ch.end());
    cout<<mp[ch[0]]<<" "<<ch[0]<<endl;
    return 0;
	}

### B - Tokitsukaze and Mahjong

#### 题目大意
和打金花一样，输入三个字符串，至少添加几个字符串能有花色数字全部相同或者数字升序，花色相同。
#### 模拟
少一种情况让我止步test 72 ,然后心态崩了，没有后来了

	int main(){
    ios::sync_with_stdio(0);
    string s[3];
    set<string> st;
    set<char> stc;
    vector<int> a;
    fro(i,0,3){
    cin>>s[i];
    st.insert(s[i]);
    stc.insert(s[i][1]);
    a.push_back(s[i][0]-'0');
    }
    bool ok=0,ok1=0,flag=0;
    string pos1,pos2;
    if(stc.size()==2){
        fro(i,0,3){
           fro(j,i+1,3){
            if(s[i][1]==s[j][1]){
                pos1=s[i];pos2=s[j];
                flag=1;
                break;
            }
           }
           if(flag==1)
                break;
        }
    }
    //cout<<pos1<<" "<<pos2<<endl;
    sort(a.begin(),a.end());
    int sum=a[0],sum2=0;
    fro(i,1,a.size()){
        if(a[i]-sum==1){
            ok=1;
            sum2++;
        }
        else if(a[i]-sum==2)
            ok=1;
        sum=a[i];
    }
    if(sum2==2)
        ok1=1;
    if(st.size()==1)
        cout<<0<<endl;
    else if(st.size()==2){
        cout<<1<<endl;
    }
    else{
        if(stc.size()==1){
            if(ok1)
                cout<<0<<endl;
            else if(ok){
                cout<<1<<endl;
                }
                else
                    cout<<2<<endl;
            
        }
        else if(stc.size()==2){
            int a=pos1[0],b=pos2[0];
            if(abs(a-b)==1||abs(a-b)==2)
                cout<<1<<endl;
            else{
                cout<<2<<endl;
            }
        }
        else
            cout<<2<<endl;
    }
    return 0;
	}

### C - Tokitsukaze and Discard Items
#### 题目大意
给出n个项目，编号为1到n，每一页有k个项目，n个项目中有m个特殊项目是要丢弃的，丢弃的方式为在最前面有特殊项目的那一页开始操作，每操作一次，所面对的那一页里的所有特殊项都会被丢弃，然后之后的项会往前移动填充这一页，如果这一页没有特殊项就要翻页去对有特殊项的页进行操作，如此操作最终把所有特殊项丢弃掉，求操作的次数。
#### 思路
通过m 来进行操作 ， n 范围太大

主要在于确定删去特殊数字后每个页面的最后一个数字。

Sum 表示记录删去特殊数字的数量

T 表示当前特殊数字的下标

A[t]表示特殊数组

那么 ((a[t]-sum-1)/k) 表示当前的页面

 ((a[t]-sum-1)/k+1)*k+sum 表示当前页面的最后一个数字

当a[t] 小于这个数字就表示它属于该页面 ， 否则就ans++;

模拟

	ll dis[maxn];
	int main(){
    ios::sync_with_stdio(0);
    ll n,m,k;
    cin>>n>>m>>k;
    fro(i,0,m){
        cin>>dis[i];
    }
    ll sum=0,ans=0,step=0;
    fro(i,0,m){
        if(dis[i]>ans){
            step++;
            ans=((dis[i]-1-sum)/k+1)*k+sum;
        }
        sum++;
    }
    cout<<step<<endl;
    return 0;
	}

## uva 437  tour DAG上的动态规划

#### 题目大意
和矩形嵌套差不多其实，就是有n个立方体，立方体每个便=边都可以做高，问最高可以堆积多高。条件是上面一个的底面长和宽严格小于下面一个立方体的上表面。

#### 思路
n很小就30，所以预处理每个立方体高的三种形式，所以最多有90个立方体。<br>

建图<br>

不固定终点的DAG动态规划，可套矩形嵌套模型，但传入参数要加个底面矩形的高。（我就是这个没搞好，涨姿势了）<br>

最后记忆化搜索解决！

	struct node{
    ll x,y,h;
    node(ll a,ll b,ll c):x(a),y(b),h(c){}
    node(){}
	}a[210];
	int n;
	bool mp[210][210];
	ll dp[210];
	int cnt=0;
	bool cheack(int aa,int b){
    node x=a[aa],y=a[b];
    return (x.x>y.x&&x.y>y.y)||(x.y>y.x&&x.x>y.y);
	}
	ll DP(ll i,ll h){
    ll &ans=dp[i];
    if(ans>0)return ans;
    ans=h;
    fro(j,0,cnt){
        if(mp[i][j])
        ans=max(ans,DP(j,a[j].h)+h);
    }
    return ans;
	}
	int main(){
    ios::sync_with_stdio(0);
    int case1=1;
    ofstream out;
    out.open("output.txt");
    while(cin>>n&&n){
            cnt=0;
        mem(a,0);mem(mp,0);
        mem(dp,0);
        fro(i,0,n){
            ll aa,b,c;
            cin>>aa>>b>>c;
            a[cnt++]=node(aa,b,c);
            a[cnt++]=node(aa,c,b);
            a[cnt++]=node(b,c,aa);
        }
       fro(i,0,cnt){
        fro(j,0,cnt){
            if(i!=j&&cheack(i,j)){
                mp[i][j]=1;
            }
        }
       }
       fro(i,0,cnt){
          DP(i,a[i].h);
       }
        ll maxa=-1,pos=0,ans=0;
        fro(i,0,cnt){
            maxa=max(maxa,dp[i]);
        }
        //cout<<maxa<<endl;
        cout<<"Case "<<case1++<<": maximum height = ";
        cout<<maxa<<endl;
    }
    out.close();
    return 0;
	}



### 去做自己喜欢的事，喜欢自己喜欢的人吧！