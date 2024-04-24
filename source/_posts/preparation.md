---
title: preparation
katex: true
date: 2024-04-22 21:16:41
excerpt: 浙大数据结构第一章，基础知识和汉诺塔实现
tag: ds
---
[7-17 汉诺塔的非递归实现](https://pintia.cn/problem-sets/15/exam/problems/821)，cout 会超时，改成 printf 不会
```cpp
#include <bits/stdc++.h>
using namespace std;
void move(char s,char m,char e,int ns){
    if (ns==1){
        //cout << s <<  " -> " << e << endl;
        printf("%c -> %c\n",s,e);
        return;
    }
    //cout << "ns=" << ns << endl;
    //s是起始柱,m是借助柱,e是目标柱
    //函数功能是将s上的ns个盘移动到e
    move(s,e,m,ns-1);   //先把ns-1个盘移动到m,再把最后一个移动到e
    move(s,m,e,1);
    move(m,s,e,ns-1);//再把m上的ns-1个移动到e
}
int main(){
    int n;
    scanf("%d",&n);
    move('a','b','c',n);
}
```

```cpp
#include <iostream>
#include <time.h>
#include <math.h>
#define MAXN 10
#define MAXK 1e8 //函数重复次数
using namespace std;

clock_t start,stop;
double duration;
double a[MAXN];

void PrintN1(int n){
    int i;
    for (i=1;i<=n;i++) cout << i << endl;
    return;
}

void PrintN2(int n){
    if (n){
        PrintN2(n-1);
        cout << n << endl;
    }
    return;
}
double f1(int n,double a[],double x){
    int i;
    double p=a[0];
    for (i=1;i<=n;i++){
        p+=(a[i])*pow(x,i);
    }
    return p;
}
double f2(int n,double a[],double x){
    double p=a[n];
    for (int i=n;i>0;i--){
        p=a[i-1]+x*p;
    }
    return p;
}

void testP2(){
    for (int n=100000;n<=100000;n*=10){
        PrintN2(n); //栈溢出
        cout << "---------------------------" << endl;
    }
}

void testP3(double a[]){
    start=clock();
    for (int i=0;i<MAXK;i++) f1(MAXN-1,a,1.1);
    stop=clock();
    duration=(double(stop-start))/CLK_TCK/MAXK;
    cout << duration << endl;

    start=clock();
    for (int i=0;i<MAXK;i++) f2(MAXN-1,a,1.1);
    stop=clock();
    duration=(double(stop-start))/CLK_TCK/MAXK;
    cout << duration << endl;


    1.01113e-06
    1.2788e-07

}

int MaxSubseqSum(int a[],int n){
        int res=0;
        int sum=0;
        for (int i=0;i<n;i++){
            sum+=a[i];
            if (sum<0) sum=0;
            if (res<sum) res=sum;
        }
        return sum;
}
int maxSubArray(int nums[], int left, int right){
    int maxLeftSum, maxRightSum;
    int maxLeftBorderSum, maxRightBorderSum;
    int leftBorderSum, rightBorderSum;

    if(left == right)
        if(nums[left] > 0)
            return nums[left];
        else
            return 0;

    int mid = (left + right) / 2, i;
    maxLeftSum = maxSubArray(nums, left, mid);
    maxRightSum = maxSubArray(nums, mid + 1, right);

    maxLeftBorderSum = 0, leftBorderSum = 0;
    for(i = mid; i >= left; i--){
        leftBorderSum += nums[i];
        if(leftBorderSum > maxLeftBorderSum)
            maxLeftBorderSum = leftBorderSum;
    }

    maxRightBorderSum = 0, rightBorderSum = 0;
    for(i = mid + 1; i <= right; i++){
        rightBorderSum += nums[i];
        if(rightBorderSum > maxRightBorderSum)
            maxRightBorderSum = rightBorderSum;
    }

    return max3(maxLeftSum, maxRightSum, maxLeftBorderSum + maxRightBorderSum);
}

int maxSubArrayAns(int nums[]){ //分治法,跨越中心的就是找到左边的最大和,找到右边的最大和再相加
    return maxSubArray(nums, 0, MAX_N - 1);
}


int main(){
    for (int i=0;i<MAXN;i++) a[i]=double(i);
    //testP3(a);

    return 0;
}

```

