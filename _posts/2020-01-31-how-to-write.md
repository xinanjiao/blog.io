---
layout: post
title: codeforce round81 div2+ uva 1617贪心
date: 2020-1-31
categories: blog
tags: [贪心,codeforce,数学]
description: 语言
---

### 说在前面
假期延长了，机票退了，聚会推了.....这也是个难得的经历，但希望也是最后一次。<br>
你说，这次疫情是报应吗？<br>
中国人信佛，外国人大多信耶稣，为什么没人信鬼神？因为我们信的都很仁慈，它能包容我们犯错误。<br>
大自然在一点意义上扮演了仁慈的角色，人类最近几十年发展迅速，基本上都是索取大自然达成的。我们最近几十年的消耗比前几十亿年的消耗总和还要多。人生病了要通过升高体温来抑制病毒，现在地球升温了，那人类就是病毒。大自然总会想办法来保护自己。<br>
科幻片里幻想着与外星人打仗，却最后都是把拥有黑洞穿越技术，不远几百万光年到达地球的外星人打败。<br>
有时候我就在想，收拾我们还用得着外星人吗？人能抗过自然吗？我们来自自然，来自自然的元素，来自自然的营养，来自自然的基因序列。自然对我们知根知底，它就在看，人类对我的索取是不是没完没了。一不注意放出一个病毒，就搞得人类鸡犬不宁。病毒在一定意义上扮演了'清道夫'的角色，清除掉对自然有压力的那部分，当然生而为人，这样说也很绝对。但大自然有它的法则，不能触碰的，绝对不能触碰，否则便会引火烧身。<br>
敬畏自然，保护自然，这才是最好的和谐生活！<br>

## codeforce round81 div2

### A - Display The Number CodeForces - 1295A 
#### 题目大意
LED灯一次只能亮n根灯管，问显示器能显示最大的数字是多少？

#### 分析
考虑大小的话，位数越多越好。考虑1(2画),7(3画)。n为偶数就输出n/2个1。n为奇数就现输出7在输出N/2-1个1

	int main(){
    ios::sync_with_stdio(0);
    int t;
    cin>>t;
    while(t--){
        int n;
        cin>>n;
        if(n%2==0){
            n=n/2;
            fro(i,0,n)
            cout<<1;
            cout<<endl;
        }
        else{
            cout<<7;
            n=n/2;
            fro(i,0,n-1)
            cout<<1;
            cout<<endl;
        }
    }
    return 0;
	}


### B - Infinite Prefixes CodeForces - 1295B 数学
#### 题目大意
给出一个由0或1组成的字符串s，用来构造出字符串 t =  s + s + s....+ s，字符串 t 由无限个 s 拼接而成。现在规定cnt_0 [ i ]和cnt_1 [ i ]分别为到第 i 位为止所出现的0和1的个数，给出一个 x ，问字符串 t 中有多少个前缀满足cnt_0 [ i ] - cnt_1 [ i ] == x，若有无限个答案，输出-1

#### 思考
没想出来.....<br>
循环的序列最后要cnt_0[i]-cnt_0[i]为x的话，无非就是:<br>
**k * (cnt_0[n]-cnt_1[n])+(cnt_0[i]-cnt_1[i])==x**<br>
其中k为全长序列重复的次数，后面就是到第i个数就满足和为x了。<br>
1. 先预处理每个位置前面（包括该位置）的0的个数和1得个数的差。
2. 将上面的式子移项得：k=(x-(cnt_0[i]-cnt_1[i]))/(cnt_0[n]-cnt_1[n])
3. 是否满足条件的话就看k了。首先cnt[n]是知道的了，如果cnt_0[n]为零就是无限前缀，但分母不能为零。
4. k必须大于等于0，且k是整数，也就是能除余。

代码

	int a[maxn];
	int main(){
    ios::sync_with_stdio(0);
    int t;
    cin>>t;
    while(t--){
        mem(a,0);
        int n,x;
        cin>>n>>x;
        string s;
        cin>>s;
        s[0]=='0'?a[0]=1:a[0]=-1;
        fro(i,1,s.size()){
            a[i]=a[i-1];
            if(s[i]=='0') a[i]++;
            else a[i]--;
        }
        n--;
        int ans=0;
        if(x==0)
            ans++;
        for(int i=0;i<=n;i++){
            if(a[n]==0){
                if(a[i]==x)
                    ans++;
            }
            else{
                if((x-a[i])%a[n]==0&&(x-a[i])/a[n]>=0)
                    ans++;
            }
        }
        if(a[n]==0&&ans)
             ans=-1;
        cout<<ans<<endl;
    }
    return 0;
	}


### C - Obtain The String CodeForces - 1295C 二分
#### 题目大意
给出一个字符串 s 和一个字符串 t ，再给出一个字符串 z ，初始时字符串 z 为空串，现在需要利用规则构造字符串 z ，使得 z == t ，规则就是每次可以挑选字符串 s 的任意子序列添加到字符串 z 后面，代价是一次操作数，问如何才能操作数最少，如果不能完成构造，输出 -1
#### 思路
预处理每个字符的位置，然后二分比较这个字符的位置是否满足在之后，如果不满足则次数加一。

	int main(){
    ios::sync_with_stdio(0);
    int t;
    cin>>t;
    while(t--){
        string s,t;
        cin>>s>>t;
        int lens=s.size(),lent=t.size();
        vector<int> a[30];
        fro(i,0,lens){
            a[s[i]-'a'].push_back(i);
        }
        bool ok=0;
        int sum=0,pos1=0,pos2=INF;
        fro(i,0,lent){
            int ch=t[i]-'a';
            if(a[ch].size()==0){
                ok=1;
                break;
            }
            else{
                int pos3=upper_bound(a[ch].begin(),a[ch].end(),pos2)-a[ch].begin();
                //cout<<pos3<<endl;
                if(pos3==a[ch].size()){
                    sum++;
                    pos3=0;
                }
                pos2=a[ch][pos3];
            }
        }
        if(ok)
            cout<<"-1"<<endl;
        else
            cout<<sum<<endl;
    }
    return 0;
	}

### Laptop UVA - 1617 贪心 区间维护
#### 题目大意
有n条长度为1的线段，确认他们的起点，使得第i条线段在ri 和 di之间，输入保证 ri <= rj 当且仅当 di<=dj 保证有解，输出空隙数目的最小值。
#### 思路
做法：<br>
设当前最后放的位置设为 r<br>
先按左端点和右端点排序。<br>
这时如果计算第一个区间，一定是放在最右端最优。<br>
如果当前这个区间的右端点与 r 相等，那么一定可以通过调节上次放的位置，然后把这次的放在 r <br>处，使得不产生间隔（题目保证有解）<br>
如果这个区间的右端点 > r，但左端点 <= r，那么我们可以放在 r+1 <br>处，使得不产生间隙（如果这次的放置会使下次产生间隙，那么就产生吧，早晚都会产生的）<br>
如果这个区间的左端点>r，那么无论如何都会产生间隙，就 cnt++，让 r 等于当前区间的右端点<br>
<p style="color: red">刚开始一直不知道cnt++怎么回事，原来就是贪心让他们尽量挨着一起</p>

	int n;
	struct node{
    int l,r;
    node(){}
    bool operator<(const node a)const{
        if(a.l==l)
            return a.r>=r;
        else
            return a.l>l;
    }
	};
	node a[maxn];
	int main(){
    ios::sync_with_stdio(0);
    ofstream out;
    out.open("output.txt");
    int t;
    cin>>t;
    while(t--){
            cin>>n;
        fro(i,0,n)cin>>a[i].l>>a[i].r;
        sort(a,a+n);
        ll sum=0;
        int top=a[0].r;
        fro(i,1,n){
            if(top>=a[i].l){
                if(top==a[i].r)
                    continue;
                top++;
            }
            else{
                sum++;
                top=a[i].r;
            }
        }
        cout<<sum<<endl;
    }
    out.close();
    return 0;
	}