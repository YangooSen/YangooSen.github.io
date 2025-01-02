---
title: BigInt
katex: true
date: 2024-12-30 21:36:37
excerpt: 大数模拟类
tag: ds
---

[1065 A+B and C (64bit)](https://pintia.cn/problem-sets/994805342720868352/exam/problems/994805406352654336)，[解答](https://blog.csdn.net/weixin_55202895/article/details/126827884)

```cpp
#include <bits/stdc++.h>

using namespace std;
const int maxn=10000;//最大长度

//高精度整数
struct BigInteger{
    //数组可以用vector,就不需要分配了
    //构造函数先resize(maxn),然后保留length作为精度
	int digit[maxn];	//存储数组
	int length;			//长度（精度）

	//构造
	BigInteger();
	BigInteger(int x);  //如果是0也要保留一个数字,len=1
	BigInteger(string str);
	BigInteger(const BigInteger& b);

	//赋值
	BigInteger operator=(int x);
	BigInteger operator=(string str);
	BigInteger operator=(const BigInteger& b);

	//比较
	bool operator<=(const BigInteger& b);
	bool operator==(const BigInteger& b);

	//运算
	BigInteger operator+(const BigInteger& b);
	BigInteger operator-(const BigInteger& b);
	BigInteger operator*(const BigInteger& b);
	BigInteger operator/(const BigInteger& b);
	BigInteger operator%(const BigInteger& b);

	//输入输出
	friend istream& operator>>(istream& in,BigInteger& b);
	friend ostream& operator<<(ostream& out,BigInteger& b);
};

BigInteger::BigInteger(){ //默认构造函数
	memset(digit,0,sizeof(digit));
	length=0;
}
BigInteger::BigInteger(int x){ //整型构造函数
	memset(digit,0,sizeof(digit));
	length=0;
	if(!x) digit[length++]=x;
	while(x){
		digit[length++]=x%10;
		x/=10;
	}
}
BigInteger::BigInteger(string str){	//字符串构造函数
	memset(digit,0,sizeof(digit));
	length=str.size();
	for(int i=0;i<length;i++){
		digit[i]=str[length-i-1]-'0';
	}
}
BigInteger::BigInteger(const BigInteger& b){ //高精度整型构造函数
	memset(digit,0,sizeof(digit));
	length=b.length;
	for(int i=0;i<length;i++){
		digit[i]=b.digit[i];
	}
}

BigInteger BigInteger::operator=(int x){ //整型赋值
	memset(digit,0,sizeof(digit));
	length=0;
	if(!x){
		digit[length++]=x;
	}
	while(x){
		digit[length++]=x%10;
		x/=10;
	}
	return *this;
}
BigInteger BigInteger::operator=(string str){	//字符串赋值
	memset(digit,0,sizeof(digit));
	length=str.size();
	for(int i=0;i<length;i++){
		digit[i]=str[length-i-1]-'0';
	}
	return *this;
}
BigInteger BigInteger::operator=(const BigInteger& b){ //高精度整数赋值
	memset(digit,0,sizeof(digit));
	length=b.length;
	for(int i=0;i<length;i++){
		digit[i]=b.digit[i];
	}
	return *this;
}

bool BigInteger::operator<=(const BigInteger& b){ //小于等于
	if(length<b.length) {
		return true;
	}
	else if(b.length<length) {
		return false;
	}
	else{
		//倒着遍历,123存储的是{3,2,1},应该从高位开始遍历
		for(int i=length-1;i>=0;i--){
			if(digit[i]==b.digit[i])  continue;
			//这里实现的是<=,应该这么写
			else return digit[i]<b.digit[i];
		}
	}
	return true;
}
bool BigInteger::operator==(const BigInteger& b){ //等于
	if(length!=b.length)
		return false;
	for(int i=0;i<length;i++){
		if(digit[i]!=b.digit[i])
			return false;
	}
	return true;
}

BigInteger	BigInteger::operator+(const BigInteger& b){	 //加法运算
	int flag=0,temp;
	BigInteger ans;
	for(int i=0;i<length || i<b.length;i++){
		temp=digit[i]+b.digit[i]+flag;
		ans.digit[ans.length++]=temp%10;
		flag=temp/10;
	}
	if(flag){	//别忘了加最后的进位carry
		ans.digit[length++]=flag;
	}
	return ans;
}
BigInteger	BigInteger::operator-(const BigInteger& b){	 //减法运算（大数减小数）
	BigInteger ans;
	int flag=0,temp;
	for(int i=0;i<length || i<b.length;i++){
		temp=digit[i]-b.digit[i]-flag;
		if(temp<0){
			temp+=10;
			flag=1;
		}
		else{
			flag=0;
		}
		ans.digit[ans.length++]=temp;
	}
	while(ans.digit[ans.length-1]==0 && ans.length>1>1) ans.length--;
	return ans;
}
BigInteger	BigInteger::operator*(const BigInteger& b){	 //乘法运算
	BigInteger ans;
	ans.length=length+b.length;
	for(int i=0;i<length;i++){
		for(int j=0;j<b.length;j++){
			ans.digit[i+j]+=digit[i]*b.digit[j];
		}
	}
	for(int i=0;i<ans.length;i++){
		ans.digit[i+1]+=ans.digit[i]/10;
		ans.digit[i]%=10;
	}
	while(ans.digit[ans.length-1]==0 && ans.length>1>1) ans.length--;
	return ans;
}
BigInteger	BigInteger::operator/(const BigInteger& b){	 //除法运算
	BigInteger remainder=0;
	BigInteger temp=b;
	BigInteger ans;
	ans.length=length;
	for(int i=length-1;i>=0;i--){
		//从高位开始除,如果余数不是0
		if(!(remainder.digit[0]==0 && remainder.length==1)){
			//移位,给落下来的y一位空出位置
			for(int j=remainder.length-1;j>=0;j--){
				remainder.digit[j+1]=remainder.digit[j];
			}
			remainder.length++;
		}
		//被除数的这一位落下来
		remainder.digit[0]=digit[i];
		while(temp <= remainder){
			remainder=remainder-temp;
			ans.digit[i]++;
		}
	}
	while(ans.digit[ans.length-1]==0 && ans.length>1) ans.length--; //全0的话要保留一个0,因此不能>=1
	return ans;
}
BigInteger	BigInteger::operator%(const BigInteger& b){ //模运算
	BigInteger remainder=0;
	BigInteger temp=b;
	for(int i=length-1;i>=0;i--){
		if(!(remainder.digit[0]==0 && remainder.length==1)){
			for(int j=remainder.length-1;j>=0;j--){
				remainder.digit[j+1]=remainder.digit[j];
			}
			remainder.length++;
		}
		remainder.digit[0]=digit[i];
		while(temp <= remainder){
			remainder=remainder-temp;
		}
	}
	return remainder;
}

istream& operator>>(istream& in,BigInteger& b){	 //输入
	string str;
	in>>str;
	b=str;
	return in;
}
ostream& operator<<(ostream& out,BigInteger& b){ //输出
	for(int i=b.length-1;i>=0;i--){
		out<<b.digit[i];
	}
	return out;
}
int main(){
	BigInteger res=BigInteger("1345")/BigInteger("5");
    cout << res << endl;
}
```
```cpp
#include<bits/stdc++.h>
using namespace std;
#define maxDigits 1e5
class bigInt{
public:
    vector<int> digits;
    int len;
    bigInt(){
        //默认初始化是空
        digits.resize(maxDigits);
        len=0;
    }
    bigInt(const string& str){
        digits.resize(maxDigits);
        len=0;
        for (int i=str.size()-1;i>=0;i--){
            digits[len++]=str[i]-'0';
        }
    }
    bool operator==(const bigInt& b)const{
        if (len!=b.len) return false;
        for (int i=0;i<len;i++){
            if (digits[i]!=b.digits[i]) return false;
        }
        return true;
    }
    bool operator<=(const bigInt& b)const{
        if (len<b.len) return true;
        if (len>b.len) return false;
        for (int i=len-1;i>=0;i--){
            if (digits[i]==b.digits[i]) continue;
            else return digits[i]<b.digits[i];
        }
        return true;    //一直continue的话就会返回true
    }
    bigInt operator+(const bigInt& b)const{
        int carry=0;
        bigInt answer;
        //123存储的是{3,2,1},应该从3开始加
        for (int i=0;i<len || i<b.len;i++){
            //resize之后的都是0,相当于已经高位对齐0了
            int current=digits[i]+b.digits[i]+carry;
            if (current>10){
                carry=1;
                current-=10;
            }
            else carry=0;
            answer.digits[answer.len++]=current;
        }
        if (carry) answer.digits[answer.len++]=carry;
        return answer;
    }
    bigInt operator-(const bigInt& b)const{
        //大数减小数,从低位开始减
        int carry=0;    //借位
        bigInt answer;
        for (int i=0;i<len || i<b.len;i++){
            int current=digits[i]-b.digits[i]-carry;
            if (current<0){
                carry=1;
                current+=10;
            }
            else carry=0;
            answer.digits[answer.len++]=current;
        }
        //去除高位0
        while (answer.digits[answer.len-1]==0 && answer.len>1) answer.len--;
        return answer;
    }
    bigInt operator*(const bigInt& b)const{
        bigInt answer;
        answer.len=b.len+len;
        for (int i=0;i<len;i++){
            for (int j=0;j<b.len;j++){
                answer.digits[i+j]+=digits[i]*b.digits[j];
            }
        }
        //进位
        for (int i=0;i<answer.len;i++){
            answer.digits[i+1]+=answer.digits[i]/10;
            answer.digits[i]%=10;
        }
        while (answer.digits[answer.len-1]==0 && answer.len>1) answer.len--;
        return answer;
    }
    bigInt operator/(const bigInt& b)const{
        bigInt tmp=b,remainder("0"),answer; //remainder初始化是0
        answer.len=len;
        //从高位开始除
        for (int i=len-1;i>=0;i--){
            if (!(remainder.digits[0]==0 && remainder.len==1)){
                //remainder不是0,就开始移位
                for (int j=remainder.len-1;j>=0;j--){
                    remainder.digits[j+1]=remainder.digits[j];
                }
                remainder.len++;
            }
            remainder.digits[0]=digits[i];
            while (tmp<=remainder){
                //看看余数里有几个被除数
                remainder=remainder-tmp;
                answer.digits[i]++;
            }
        }
        while (answer.digits[answer.len-1]==0 && answer.len>1) answer.len--;
        return answer;
    }
    bigInt operator%(const bigInt& b)const{
        bigInt tmp=b,remainder("0");
        for (int i=len-1;i>=0;i--){
            if (!(remainder.digits[0]==0 && remainder.len==1)){
                //remainder不是0,就开始移位
                for (int j=remainder.len-1;j>=0;j--){
                    remainder.digits[j+1]=remainder.digits[j];
                }
                remainder.len++;
            }
            remainder.digits[0]=digits[i];
            while (tmp<=remainder){
                //看看余数里有几个被除数
                remainder=remainder-tmp;
            }
        }
        return remainder;
    }
};
ostream& operator<<(ostream& out,const bigInt& b){
    for (int i=b.len-1;i>=0;i--) out << b.digits[i];
    return out;
}

int main(){
    int n,m;
    bigInt inf("1"),two("2");
    for (int i=0;i<10;i++) inf=inf*two;
    cout << inf << endl;

}
```

