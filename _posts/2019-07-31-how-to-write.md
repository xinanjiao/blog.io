---
layout: post
title: ACM训练第9天
date: 2019-7-31
categories: blog
tags: [算法,训练,二分+贪心]
description: 语言
---
## 写在前面
今天B组比赛队友发挥还行，我就划水了，哈哈哈，但后面出了点意外,,,,但今天我们的表现还是可圈可点的，配合也更加成熟了，虽然今天死活没有A出B题，但是我后来看一看里面的贪心+二分这道题，恰好最近的专题训练也是贪心+二分，所以特地学了学，（题做过越多则越熟练！！！)

## 贪心+二分（代码后补）
### Aggressive cows （POJ 2456）
题目链接：<https://vjudge.net/problem/POJ-2456><br/>
枚举答案，在贪心寻找<br/>


### Pie（POJ 3122)
题目链接:<https://vjudge.net/problem/POJ-3122><br/>
把蛋糕最多均分给m+1个人，问最大的分法，但是注意只能同一个蛋糕分，不能不同蛋糕凑成一个体积<br/>


### Cable master(POJ 1064)
题目链接:<https://vjudge.net/problem/POJ-1064><br/>
注意输出时的floor()函数，为向下取整函数，目的时因为题目有限制，一根木棒最多切100次<br/>


### K Best （POJ 3111）
题目链接：<https://vjudge.net/problem/POJ-3111><br/>
这道题时均分值问题的升级版，均分问题只叫输出均分的最大数值，而这是问你选哪几个编号，这就要开结构体储存标号<br/>


### Strange fuction (HDU 2899)
题目链接：<https://vjudge.net/problem/HDU-2899><br/>
给你一个计算公式，求他当x为1到100时的最大值，由数学公式知道，它是递增函数，就二分1到100，求导，二分+贪心,注意二分跳出条件1e-6<br/>









