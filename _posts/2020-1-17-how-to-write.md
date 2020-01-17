---
layout: post
title: uva1611 11925 构造 思维
date: 2020-1-17
categories: blog
tags: [构造,uva]
description: 语言
---

《构造》，一种简单且不好想的方法，这两道题有个共同点，都规定操作次数，在规定的操作次数内完成操作。不要求最优解问题。综上所述这种题多半是构造题。构造很考脑筋，构造得好代码也很简单，构造得不好或者没有想到构造就会像我一样情不自禁的去想n的n方的算法（hhhh)，说多无益，这也是个多做多总结的过程。

### Crane UVA - 1611 构造
#### 题目大意
输入一个1 ~ n(1<=n<=10000)的排列，用不超过9^6次操作把它变成升序。每次操作都可以选一个长度为偶数的连续区间，交换前一半和后一半。
#### 题解
9的6次方，大概是5e5这么大吧。暴力变化（偶数区间为2）需要!n的复杂度，这肯定不行啊。紫书友情提示：可以在2n的时间内完成。这什么意思？也就是说每个数移动两次就可以到达正确的位置，怎么会那么快，那就得在长度为偶数的区间内交换下功夫。
<p style="color: red">这道题是排序，解法和选择排序类似，也和前面摊煎饼那道题类似，排序的时候，前面i个数排好来就不用管了</p>
设数i不在原位i而在其大于i的位置pos，我们可以知道，最快将pos移动到i处就是：i在偶数区间最左边,pos在中间偏右一个位置，这样就可一步到位。

但这样对区间的要求就是(pos-i) * 2+i-1<=n。(pos-i) * 2是区间的长度，i-1是前面i个排好的数，看数组长度是否满足区间交换长度，如果不满足看下面：<br>

如果pos-i为奇数就逆置[i,pos]区间的数。反之就逆置[i+1,pos]区间的数，在接着上面个步骤判断。

    int a[maxn];
    vector<P> s;
    void change(int l,int r){
    s.push_back(make_pair(l,r));
    int mid=l+(r-l)/2+1;
    for(int i=l,j=mid;j<=r;i++,j++){
        swap(a[i],a[j]);
    }
    }
    int main(){
    ios::sync_with_stdio(0);
    int t;
    cin>>t;
    while(t--){
        s.clear();
        int n;
        cin>>n;
        int pos;
        fro(i,1,n+1){
            cin>>a[i];
        }
        for(int i=1;i<=n;i++){
            for(int j=i;j<=n;j++){
                if(a[j]==i){//匹配位置与数
                    pos=j;
                    break;
                }
            }
            if(pos==i)continue;
            if((pos-i)*2+i-1<=n){
                change(i,2*pos-i-1);
            }
            else{
                if((pos-i)%2==1)
                    change(i,pos);
                else
                change(i+1,pos);
                i--;
            }
        }
    int len=s.size();
    cout<<len<<endl;
    fro(i,0,len){
      cout<<s[i].first<<" "<<s[i].second<<endl;
    }
    }
    return 0;
    }

### Generating Permutations UVA - 11925 构造 贪心
#### 题目大意
由一个排列，给两种操作（1，交换头两个数字；2，将第一个数字放到最后）。问从由一个升序原始序列1，2，3...n变成给定排列的操作序列是什么？

#### 思考
紫书上解释反了，只不过影响不是很大，卡在调试代码上，没卡在题意上......最后实际上逆向思考一下，将最后一个数字放在第一个就行了，在逆序输出操作编号<br>
贪心思路：<br>
说是构造吧，他也是，但我觉得是贪心的思路。又是排序有木有，而且题目中说最多交换次数为2 * n^2。构造无疑了。解决这个问题就要想想排序的本质是什么，他又没有要求最优交换次数。排序的本质就是**交换**。交换就要有比较基准，排序就是和比较基准比较和交换的过程。我们保证每次操作都执行2操作，是否执行1操作比较a[0]和a[1]的大小(这就算个贪心叭),交换和比较。(注意a[0]不能为最大的那个数)



    int a[310];
    vector<int> s;
    bool cheak(int n){
    for(int i=0,j=1;i<n;i++,j++)
        if(a[i]!=j)
           return false;
    return true;
    } 
    void test(int n){
    fro(i,0,n)
    cout<<a[i];
    cout<<endl;
    }
    int main(){
    ios::sync_with_stdio(0);
    int n;
    while(cin>>n&&n){
        fro(i,0,n)
        cin>>a[i];
        s.clear();
        while(1){
            if(a[0]!=n&&a[0]>a[1]){
                s.push_back(1);
                swap(a[0],a[1]);
                if(cheak(n))
                    break;
            }
            s.push_back(2);
            int t=a[n-1];
            for(int i=n-1;i>=1;i--)
                a[i]=a[i-1];
            a[0]=t;
            //test(n);
            if(cheak(n))
                break;
        }
        for(int i=s.size()-1;i>=0;i--)
        cout<<s[i];
        cout<<endl;
    }
    return 0;
    }




