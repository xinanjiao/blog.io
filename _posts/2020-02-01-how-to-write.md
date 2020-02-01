---
layout: post
title: codeforce round570 div3 +uva  1618 技巧枚举
date: 2020-02-01
categories: blog
tags: [codeforce,枚举]
description: 语言
---

## codeforce round570 div3 持续更新

### A - Nearest Interesting Number

#### 题目大意
给出一个数n，(n<=1000)。找出靠近n最大的一个数，这数每一位的数字之和能被四整除。

#### 思路
暴力即可，因为肯定存在

	bool cheack(int n){
    int a=n;
    int sum=0;
    while(a!=0){
        int ans=a%10;
        sum+=ans;
        a/=10;
    }
    if(sum%4==0)
        return true;
    else
        return false;
	}
	int main(){
    ios::sync_with_stdio(0);
    int n;
    cin>>n;
    for(int i=n;;i++){
        if(cheack(i)){
            cout<<i<<endl;
            break;
        }
    }
    return 0;
	}

### B - Equalize Prices
#### 题目大意
给出n个数字，和一个数字k。要求得出一个最大平均价格B,每个数转换为B可以最多加或减k个数。
#### 思路
暴力肯定超时。考虑最大值和最小值。如果最小值加于k小于最大值减掉k，那就不可能有最大值B。反之输出最小值加K。

	int main(){
    ios::sync_with_stdio(0);
    int t;
    cin>>t;
    while(t--){
        int n,k;
        cin>>n>>k;
        int mina=INF,maxa=-1;
        fro(i,0,n){
            int a;
            cin>>a;
            mina=min(a,mina);
            maxa=max(maxa,a);
        }
        if(mina+k<maxa-k)
            cout<<-1<<endl;
        else{
            cout<<mina+k<<endl;
        }
    }
    return 0;
	}

### C - Computer Game 二分

#### 题目大意
题读了好久好久，没读懂。经过队友一说，原来是题目中的（the first type turn)理解错了。<br>
给出一个数k，操作数为n，一个数a和一个数b，满足（a>b)。要求使用a和b的次数不能大于n，且最后的k要大于0<br>
要求求得最大的a的使用次数，满足上述条件。<br>

#### 思路
原始思路还是枚举，改了半天代码，最后TLE test15，意料之中。枚举不行，试试二分，二分答案。

	int main(){
    //ios::sync_with_stdio(0);
    int t;
    cin>>t;
    while(t--){
        ll k,n,a,b;
        cin>>k>>n>>a>>b;
        if(b*n>=k)
            cout<<-1<<endl;
        else{
            ll l=0,r=n,mid,ans;
            while(l<=r){
                 mid=l+(r-l)/2;
                if(a*mid+b*(n-mid)<k){
                    l=mid+1;
                    ans=mid;
                }
                else
                    r=mid-1;
                    //cout<<l<<" "<<r<<endl;
            }
            cout<<ans<<endl;
        }
    }
    return 0;
	}


### D - Candy Box (easy version) 
#### 题目大意
给出n种类型的糖果。要求准备礼物，满足条件：礼物中不同类型的糖果的数量必须不同。比如：两个8号糖果和两个5号糖果就不行。求最多一个礼物能有多少糖果。
#### 思路
先桶排序得到每个数字的重复次数，贪心从最多重复次数的类型糖果开始装，然后数量一直减减，这样遍历储存重复次数的数组。<br>
如果不注意，在找每个数字的重复次数的时候会超时。因为桶排序不知道数组里哪些有数据而哪些没有，找数据时只能遍历整个数组，浪费时间。<br>
所以我考虑：桶排序数组里有用的地方只到max_element(a,a+n)，即输入的数据中最大的那个数那里。从大到小对桶排个序，有用的数据就前面不重复元素的个数。<br>

最后我遇到了我以前中的坑，就是，memset()超时问题，嘿嘿。最后改过来就A了。

	int a[maxn];
	vector<int> num;
	int main(){
    ios::sync_with_stdio(0);
    int q;
    cin>>q;
    while(q--){
        int n;
        cin>>n;
        fro(i,0,n+1)
         a[i]=0;
       num.clear();
        int maxa=-1;
        set<int> st;
        fro(i,0,n){
            int t;
            cin>>t;
            st.insert(t);
            maxa=max(maxa,t);
            a[t]++;
        }
        int len=st.size();
        sort(a,a+maxa+1,greater<int>());
        fro(i,0,len){
            if(a[i])
               num.push_back(a[i]);
        }
        sort(num.begin(),num.end(),greater<int>());
        int sum=0;
        int pos=num[0];sum+=pos;
        pos--;
        fro(i,1,num.size()){
            if(pos==0)
                break;
            if(num[i]>=pos){
                sum+=pos;
            }
            else{
                pos=num[i];
                sum+=pos;
            }
            pos--;
        }
        cout<<sum<<endl;
    }
    return 0;
	}

### Weak Key UVA - 1618 技巧枚举
#### 题目大意
对于一个长度为n的序列，找到四个数，Np,Nq,Ns,Nr(p>q>s>r)使得Nq>Nr>Np>Ns或Nq<Nr<Np<Ns。
#### 思路
当初的想法肯定超时，就不说了。<br>
参考了网上的做法。<br>
枚举Nq和Ns，因为他们一个最大一个最小，<br>
预处理每个数左边小于它的数，和它右边大于它的数，记录。<br>
每次枚举完Nq和Ns后。二分查找Nq当中大于Ns的数Np。二分查找Ns右边小于Nq的数Nr。如果满足Nr>Np就可以。

	int n;
	vector<int> a;
	bool cheak(){
    vector<int> l[5100],r[5100];
        fro(i,0,n){
            int ans=a[i];
            for(int j=i-1;j>=0;j--){
                if(a[j]<ans)
                    l[i].push_back(a[j]);
            }
            for(int k=i+1;k<n;k++){
                if(a[k]>ans)
                    r[i].push_back(a[k]);
            }
            sort(l[i].begin(),l[i].end());
            sort(r[i].begin(),r[i].end());
           // cout<<"???"<<endl;
        }
        for(int i=0;i<n;i++){
               int ans=a[i];
            for(int j=i+1;j<n-1;j++){
                if(a[j]<ans){
                    if(l[i].size()==0||r[j].size()==0)
                        continue;
                    int p=upper_bound(l[i].begin(),l[i].end(),a[j])-l[i].begin();
                    int q=lower_bound(r[j].begin(),r[j].end(),ans)-r[j].begin();
                    if(p==l[i].size()||q==0)
                        continue;
                     if(r[j][q-1]>l[i][p])
                        return true;
                }
            }
        }
        return false;
	}
	int main(){
    ios::sync_with_stdio(0);
    int t;
    cin>>t;
    while(t--){
        cin>>n;
        a.clear();
        fro(i,0,n){
            int aa;
            cin>>aa;
            a.push_back(aa);
        }
        bool ok=0;
        if(cheak())
            ok=1;
        reverse(a.begin(),a.end());
        if(cheak())
            ok=1;
        ok==1?cout<<"YES"<<endl:cout<<"NO"<<endl;
    }
    return 0;
	}

