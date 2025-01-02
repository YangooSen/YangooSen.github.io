---
title: FastExponentiation
katex: true
date: 2024-12-30 21:34:18
excerpt: 快速幂算法
tag: ds
---

来自[快速幂](https://blog.csdn.net/qq_19782019/article/details/85621386)
```cpp
#include <iostream>
using namespace std;

//((a%p)*(b%p))=(a*b)%p
long long fastPower(long long base, long long power) {
    long long result = 1;
    while (power > 0) {
        if (power&1) {  //奇数的最后一位是1,和1与之后是奇数返回1
            power-=1;                  //把指数减去1，使其变成一个偶数
            result=result*base%1000;      //此时记得要把指数为奇数时分离出来的底数的一次方收集好
            power>>=1;               //把指数缩小为一半
            base=base*base%1000;        //底数变大成原来的平方
        }
        else {
            //如果指数为偶数
            power>>=1;                   //把指数缩小为一半
            base=base*base%1000;        //底数变大成原来的平方,收集在base中
        }
    }
    return result;
}

int main(){
    cout << fastPower(2,1033);
}


```

