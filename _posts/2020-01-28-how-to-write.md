---
layout: post
title: uva1616 二分+贪心
date: 2020-1-28
categories: blog
tags: [二分+贪心]
description: 语言
---

### 收获
二分+贪心，这种题型前面我也归纳了一下，现在见到这种题也就很容易能想到。只不过这道题的小数精度要考虑到1e-10，普通1e-6或1e-8就可以了，这道题卡精度还是比较严。<br>

还有就是这道题最后的变态输出：要求转换为分数输出<br>

我刚开始想得很蛇皮。我记得当时学c语言做oj的时候就有将小数转换为整数的题。<br>

我的思路就是：管他那么多，先将最后的小数都乘以1e9，然后再转换为long long。边除边判断，当分母（分母初始化为1e9）为1或除余的结果不为0时，退出循环。<br>

可能这种奇淫异巧，对oj那种小数据还有用，但毕竟这道题的精度卡到了1e-10。当一个double 的数，比如3.33。乘以1e9时，就为333000000001,显然double 有精度损失。方法不可行。<br>

参照网上的方法：枚举分母，得分子，进而比较求得最小。（为什么枚举上限位线段个数，我也很迷）。

<p style="color: red"> round()函数和ceil 与floor不同的是，它返回的是小数的四舍五入结果</p>

	int rp=0,rq=1;//rp分子,rq分母
        for(int p,q=1;q<=n;q++){
            p=round(l*q);//round四舍五入
            if(fabs(1.0*p/q-l)<fabs(1.0*rp/rq-l)){
                rp=p,rq=q;
            }
        }
        cout<<rp<<"/"<<rq<<endl;



### Caravan Robbers UVA - 1616 二分+贪心

#### 题目大意
给n个互不相包含的区间，求出一个长度的最大值，使得可以在每个区间中选出这样一个长度的子区间，这些子区间互不相交。结果用分数表示

#### 思路
二分+贪心，get小数转换为分数

	const int maxn = 1e5+100;
	const int hashmaxn=8388608;
	int lowbit(int x){return x&(-x);}
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
	bool judge(double ans){
    double pos=a[0].l+ans;
    fro(i,1,n){
        if(a[i].l<=pos){
            if(pos+ans-a[i].r>EPS)
            return false;
          else
            pos=pos+ans;
        }
        else if(a[i].l-pos>EPS)
            pos=a[i].l+ans;
    }
    return true;
	}
	int main(){
    ios::sync_with_stdio(0);
    ofstream out;
    out.open("output.txt");
    while(cin>>n){
            int mina=INF;
        fro(i,0,n){
            cin>>a[i].l>>a[i].r;
            mina=min(mina,a[i].r-a[i].l);
        }
        sort(a,a+n);
        double l=0,r=mina,mid;
        while(r-l>=EPS){
             mid=l+(r-l)/2.0;
            if(judge(mid)){
                l=mid;
            }
            else{
                r=mid;
            }
        }
        //cout<<l<<endl;
        //最优转分数
        int rp=0,rq=1;
        for(int p,q=1;q<=n;q++){
            p=round(l*q);
            if(fabs(1.0*p/q-l)<fabs(1.0*rp/rq-l)){
                rp=p,rq=q;
            }
        }
        cout<<rp<<"/"<<rq<<endl;
    out.close();
    }
    return 0;
	}







