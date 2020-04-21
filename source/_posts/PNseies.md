---
title: PN序列的产生以及相关函数的计算
date: 2020-3-23 16:52:24
updated: 2020-3-23 16:52:24
tags: 
  - 通信
categories:
  - 专业课
---

﻿ 1. 求 PN 序列并极化（给定初始序列[1 0 0 0 0 0]），c(n)代表序列第 n 个值，c(0)代表 c(N)
```cpp
function cout=c(L,n)
    head=[[1 0 0 0 0 1],%[6,1]
          [1 1 0 0 1 1],%[6,5,2,1]
          [1 0 0 1 1 1]];%[6,5,4,1]
    head_L=logical(head(L,:));
    cc=[1 0 0 0 0 0];
    c_t=[];
    N=63;
    n=mod(n,N);
    if(n==0)
        n=n+N;
    end
    for i=1:n-1
       temp=cc(head_L);
       c_t(1)=mod(sum(temp),2);
       c_t(2:6)=cc(1:5);
       cc = c_t;
    end
    if(cc(6)>0)
        cout = 1;
    else
        cout = -1;
    end
```
2. 求相关函数，Rc1(L1,L2)是指求 L1 抽头和 L2 抽头的互相关函数，L1=L2 则为自相关函数 
	L1:[6,1]

	L2:[6,5,2,1]
 
	L3:[6,5,4,1]
	

```cpp
function [Tao,RC]=Rc1(L1,L2)
Rc=[];
Rc_T=0;
tao=-50:50;
for(i=-50:50)
    for t=-31:31
        Rc_T =Rc_T+c(L1,t)*c(L2,t-i);
    end
    Rc(i+51)=Rc_T/63;
    Rc_T=0;
end
Tao=tao;
RC=Rc;
plot(tao,Rc);
```
 

 3. 实际计算相关函数
 

```cpp
%%计算自相关函数
%[6,1]
[tao,Rc61]=Rc1(1,1);%Rc1是求相关函数，（1,1）则是[6，1]的自相关函数
%[6,5,2,1]
[tao,Rc6521]=Rc1(2,2);
%%计算互相关函数
[tao,cross1]=Rc1(1,2);%Rc1是求相关函数，（1,2）则是[6,1]和[6,5,2,1]的互相关函数
[tao,cross2]=Rc1(2,3);
figure(1);
subplot(1,3,1);plot(tao,Rc61);title("[6,1]的自相关函数");xlabel("τ");ylabel("Rc(τ)");
subplot(1,3,2);plot(tao,Rc6521);title("[6,5,2,1]的自相关函数");xlabel("τ");ylabel("Rc(τ)");
subplot(1,3,3);plot(tao,cross1*63);axis([-60 60 -20 20]);title("[6,1]和[6,5,2,1]的互相关函数");xlabel("τ");ylabel("Rc(τ)");
figure(2);
plot(tao,cross2*63);axis([-60 60 -20 20]);title("[6,5,4,1]和[6,5,2,1]的互相关函数");xlabel("τ");ylabel("Rc(τ)");
display(max(abs(cross1)*63));
display(max(abs(cross2)*63));
```
输出结果：
![figure1](https://img-blog.csdnimg.cn/20200318143708855.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTY3MjgyOA==,size_16,color_FFFFFF,t_70)
![figure2](https://img-blog.csdnimg.cn/20200318143748900.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTY3MjgyOA==,size_16,color_FFFFFF,t_70)
这是数字通信课程的一道编程作业题，这里求相关函数采用的是循环，还有很大优化空间，要注意的是序列周期延拓各个下标的值怎么映射到原始序列，这其实就是一个求模的过程，这里互相关函数figure2最大值为15，figure1最大值为17，figure2中两个抽头产生的PN序列可用来产生gold序列，他们的互相关函数不会超过
$$
2^{(n+1)/2}+1\,（n为奇数）
$$
$$
2^{(n+2)/2}+1\,（n为偶数）
$$

