---
layout: post
title: codeforce round570 div3 +uva  1618 技巧枚举
date: 2020-02-01
categories: blog
tags: [codeforce,枚举,动态规划]
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

## 2.02更新

### E - Subsequences (easy version) BFS求不重复子序列
#### 题目大意
长度为n的字符串s(1<=n<=100)。能否组成k个不重复子序列（1<=k<=100)。（空字串也算）。能就输出组成k个字串的最小代价（删掉字符的个数最小）。否则输出-1。

#### 思路
自己想的时候并没有很好的办法。但对于小数据的这道题来说，所以复杂度就取决于k。<br>
因为字符串s可以有的组合字串方式有2^n次方种。2^100次方是很大的。<br>
但针对这道题，当枚举数量到达k时就可以退出循环，与下一道题不同。<br>

所以采用BFS暴力构造字符串加上set判重来处理。<br>

	int main(){
    ios::sync_with_stdio(0);
    int n,k;
    cin>>n>>k;
    string s;
    cin>>s;
    queue<string> q;
    set<string> st;
    q.push(s);
    st.insert(s);
    ll sum=0;
    while(!q.empty()&&st.size()<k){
        s=q.front();
        q.pop();
        fro(i,0,s.size()){
            string ans=s;
            ans=ans.erase(i,1);
            if(!st.count(ans)&&st.size()+1<=k){
                st.insert(ans);
                sum+=(n-ans.size());
                q.push(ans);
            }
        }
    }
    //cout<<st.size()<<endl;
    if(st.size()<k)
        cout<<-1<<endl;
    else
        cout<<sum<<endl;
    return 0;
	}

### H - Subsequences (hard version) （dp求字符串的不重复子序列个数）
#### 题目大意
与上面道题相同。但不同的是(1<=k<=10^2)。

### 思路
k的猛增就告诉我们，不能暴力了。<br>
这道题是道典型的字符串dp，明天打算开始学dp那一章。<br>
这道题看了很多博客，很多讲解，可能接触dp不多，理解不是很深刻。先转一个很好的讲解，等后面熟练一点了在细细品味！<br>
下面讲解的链接：<https://blog.csdn.net/trany_lin/article/details/88358625><br>
本题讲解即代码：<https://blog.csdn.net/mmk27_word/article/details/93999633><br>
如何递推dp[i][j],下面这个不完全对的式子还是容易想到的:

dp[i][j]=dp[i-1][j]+dp[i-1][j-1]

dp[i-1][j]存储的是前i-1个字符里删除了j个之后能形成的字串个数，那如果我不删除第i个字符,前i个字符就还是只删除了j个字符,dp[i-1][j]的值就可以累加到dp[i][j]上；

dp[i-1][j-1]存储的时前i-1个字符删除了j-1个后能形成的字串个数，如果我接着把第i个字符删除,就相当于前i个字符删除了j个,那

dp[i-1][j-1]的值就可以累加到dp[i][j]上;

可是看似正确，会有重复计算

比如qwabda 一共6个字符长,这里假设递推式下标从1开始，dp[6][3]=dp[5][2]+dp[5][3];

而dp[5][2]是前5个里删2个，这肯定包括了删除第4第五个字符bd这种情况,然后再把第6个删除形成dp[6][3]的一员;

这相当于删除了bda 如右所示 qwabda

dp[5][3]里肯定包括了删除第3,4,5个字符的情况,如右所示 qwabda;

而这两种删除方式都会留下 qwa这个字串，说明有重复情况被累加了;

如何减去重复的次数,每当递推到一个dp[i][j]时,就从第i-1到第1个字符里倒着找有没有等于第i个字符的，找到第一个后停下，假设这个位置是k,那么[k,i-1]和[k+1,i]这两种删除方式得到的剩下的串就是相同的，参考上面红色的部分，而且从[k+1,i-1]这段是两种删除方式中都会删掉的,所以[k,i]这段里有i-k个字符被删掉了，要满足dp[i][j],就还需要从[1,k-1]这段里面删掉(j-(i-k))个字符，而

dp[k-1][j-(i-k))]就是从前k-1个字符中删除(j-(i-k))个字符能形成多少不同字串，这个值贡献到了dp[i][j]的计算中，而且这个值在计算dp[i][j]累加时被加了两次，现在减掉一倍的它就可以了,然后就break;去递推下一个dp[i][j+1]或者是该dp[i+1][j]；


	ll dp[110][110];
	int pre[30];
	char s[110];
	int main(){
    //ios::sync_with_stdio(0);
    ll n,k;
    scanf("%lld%lld",&n,&k);
    scanf("%s",s+1);
    //dp[i][j]代表前i位，是否删掉j个字母
    dp[0][0]=1;
    for(int i=1;i<=n;i++){
            int a=s[i]-'a';
            dp[i][0]=1;
        for(int j=1;j<=i;j++){
            dp[i][j]=dp[i-1][j-1]+dp[i-1][j];
            if(pre[a]&&j>=(i-pre[a]))
                dp[i][j]-=dp[pre[a]-1][j-(i-pre[a])];
            dp[i][j]=min(dp[i][j],k);//防止越界
        }
        pre[a]=i;
    }
    ll sum=0;
    for(int i=0;i<=n;i++){
        sum+=min(dp[n][i],k)*i;
        k-=dp[n][i];
        if(k<=0)
            break;
    }
    if(k>0)
        cout<<-1<<endl;
    else
        cout<<sum<<endl;
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

