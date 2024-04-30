---
title: summary
katex: true
date: 2024-04-26 21:06:46
excerpt: 总结算法题中一些常见的小技巧
tag: ds
---


# 1.输入输出控制
- **大量输入输出导致超时，可以试试把 cin 和 cout 换成 scanf 和 printf**，如果涉及到 string 类可以补加下面的语句使 cin 和 cout 的速度与 scanf 和 printf 类似
```cpp
ios::sync_with_stdio(false);
cin.tie(0),cout.tie(0);
```
- 读入无停止标识符的字符串
```cpp
//输入 x a abc ..
string str;
while (cin >> str){
	//else
}
```
- 逐字符读入带空格的字符串
```cpp
char c='s';
while (c!='\n'){
	//else
	c=getchar();
}
```
- 使用 getchar() 的时候要注意上次 scanf() 读剩下的 \n 和每行结束后的 \n
- 保留小数
```cpp
#include <iomanip>
cout.setf(ios::fixed);cout.precision(n) //保留n位小数
cout.precision(n)					  //保留n位有效数字
```
- vector 一边删除一边遍历
```cpp
for (auto x=v.begin();x!=v.end();){//这里没有x++
    valid=false;
	//...对valid赋值
    if (!valid) x=v.erase(x);
    else x++;
}
```
# 2.字符串控制

- 分词时如果要求为非空格和字母，将所有非空格的内容合并为一个空格：
```cpp
if (isalnum(s[i])) topic+=tolower(s[i]);
else if (!topic.empty() && topic.back()!=' ') topic+=' ';
```
- [字符串转数字：stoi/stod/atof](https://blog.csdn.net/qq_45949701/article/details/121310639?ops_request_misc=&request_id=&biz_id=102&utm_term=stod&utm_medium=distribute.pc_search_result.none-task-blog-2~all~sobaiduweb~default-0-121310639.142%5Ev52%5Econtrol,201%5Ev3%5Eadd_ask&spm=1018.2226.3001.4187) ，1069 The Black Hole of Numbers 这道题中是要求21能转成 0021 和 2100，这类型需要注意不能用to_string 和 stoi 的转换（只能转成 21 和 12）
- 大小写转换：
```cpp
char c='A';
c=tolower(c);
string str="aBCD";
str[0]=toupper(str[0]);
```
- 判断字符类型：[isalpha、isalnum、isdigit、islower、isupper](https://blog.csdn.net/qq_16488989/article/details/120156135?ops_request_misc=&request_id=&biz_id=102&utm_term=c%20%20isalpha&utm_medium=distribute.pc_search_result.none-task-blog-2~all~sobaiduweb~default-1-120156135.142%5Ev56%5Epc_rank_34_queryrelevant25,201%5Ev3%5Eadd_ask&spm=1018.2226.3001.4187)
- 在字符串中寻找特定字符或字符串的起始下标，可以用 find 和 [npos](https://blog.csdn.net/sinat_31608641/article/details/107247452?ops_request_misc=%257B%2522request%255Fid%2522%253A%2522167030276116782412558642%2522%252C%2522scm%2522%253A%252220140713.130102334..%2522%257D&request_id=167030276116782412558642&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~top_positive~default-1-107247452-null-null.142%5Ev67%5Econtrol,201%5Ev4%5Eadd_ask,213%5Ev2%5Et3_control2&utm_term=string::npos&spm=1018.2226.3001.4187)
```cpp
#include <bits/stdc++.h>
using namespace std;

int main(){
    string s="hello world";
    int found=s.find("wol");
    if (found==string::npos) printf("Not found!\n");
    else printf("found in %d",found);
    found=s.find("wor");
    if (found==string::npos) printf("Not found!\n");
    else printf("found in %d",found);
}
```
- 下面是对字母循环平移 pos 位，利用了 a % m = ( a + m ) % m

```cpp
s[i]=(s[i]-'A'-pos+26)%26+'A';
```
- [1093 Count PAT's](https://pintia.cn/problem-sets/994805342720868352/exam/problems/994805373582557184)，下面的第一个是暴力解法，超时拿不到另一半分，另一个是计数解法AC，当时以测试案例和PAATAAT不断调整写出的，主要想法是算每个A后面有几个T，就能得到AT的个数（acnt），然后累积每个字母后面AT的个数（asum），只要是P就可以组成PAT
```cpp
#include <bits/stdc++.h>
using namespace std;

int main(){
    string s,tmp;
    cin >> s;
    long long cnt=0;
    for (int i=0,len=s.size();i<len;i++){
        if (s[i]=='P'){
            for (int j=i+1;j<len;j++){
                if (s[j]=='A'){
                    for (int k=j+1;k<len;k++){
                        if (s[k]=='T') cnt++;
                    }
                }
            }
        }
    }
    cout << cnt%1000000007;
}

#include <bits/stdc++.h>
using namespace std;

int main(){
    string s;
    getline(cin,s);
    //选取acnt[i]是s[i]的a达到的at的个数
    vector<int> acnt(s.size(),0);
    vector<int> tcnt(s.size(),0);
    vector<int> asum(s.size(),0);
    long long cnt=0;
    for (int i=s.size()-2;i>=0;i--){
        if (s[i+1]=='T') tcnt[i]=tcnt[i+1]+1;
        else tcnt[i]=tcnt[i+1];

        if (s[i]=='A'){
            //找后面有几个T
            acnt[i]=tcnt[i];
        }
        else acnt[i]=acnt[i];

        asum[i]=asum[i+1]+acnt[i];
    }

    for (int i=0,len=s.size();i<len;i++){
        if (s[i]=='P') cnt+=asum[i];
    }
    //for (auto x:asum) cout << x;
    cout << cnt%1000000007;
}
```
# 3.排序
- map 根据 second 排序，**注意如果cmp函数有歧义，sort 函数会抛出数组越界的错误**
```cpp
bool cmp(const pair<>& lhs, const pair<>& rhs) {
	return lhs.second < rhs.second;
}
int main(){
	vector<pair<>> v(m.begin(),m.end());
	sort(v.begin(), v.end(), cmp);
	return 0;
}
```
- 按照输入顺序输出记录的重复字符下标，设置 rank
```cpp
#include <bits/stdc++.h>
using namespace std;
int main(){
    string s;
    while (cin >> s){
        unordered_map<char,vector<int>> m;
        map<int,char> rank; //记录输出的顺序
        for (int i=0,len=s.size();i<len;i++){
            if (m.find(s[i])==m.end()) rank[m.size()]=s[i];
            m[s[i]].push_back(i);
        }
        //按照输入顺序输出
        //for (auto x:rank) cout << x.first << ' ' << x.second << endl;
        for (auto x:rank){
            //cout << m[x.second].size();
            if (m[x.second].size()>1){
                printf("%c:%d",x.second,m[x.second][0]);
                for (int i=1,len=m[x.second].size();i<len;i++) printf(",%c:%d",x.second,m[x.second][i]);
                printf("\n");
            }
        }
    }
}
```
# 4.数字处理类
- 涉及数字每位上的处理，**先观察一下能否简化**，比如 【abc和bcc是两个三位数，且有abc+bcc=532，所有这类数】，abc+bcc=100a+110b+12c=532即可，不需要把所有数都放上去再提取每位的数字再转换
- [\[编程题\]进制转换](https://www.nowcoder.com/questionTerminal/0337e32b1e5543a19fa380e36d9343d7) 涉及到了字符串模拟数字和大数除法，下面的数字模拟是关键，取模的时候可以只用字符串数字的最后一位模2，结果与整个字符串数字模2等价，[大数加减乘除模拟](https://blog.csdn.net/weixin_52812620/article/details/128434511?spm=1001.2014.3001.5502) 中除法较难，下面的 divide 在除数较简单时可以代替，其他操作很简单
```cpp
#include <bits/stdc++.h>
using namespace std;
string divide(string str,int x){
    //模拟竖式除法
    int remainder=0;
    for (int i=0,len=str.size();i<len;i++){
        int current=remainder*10+str[i]-'0';  //关键
        str[i]=current/x+'0';  //实现除法
        remainder=current%x;
    }
    int pos=0;
    while (str[pos]=='0') pos++;    //除去前导0
    return str.substr(pos);
}
//模拟乘法类似
string multiple(string str,int x){
    int carry=0;
    for (int i=str.size()-1;i>=0;i--){
        int current=x*(str[i]-'0')+carry;
        str[i]=current%10+'0';
        carry/=10;
    }
    if (carry) str='1'+str;
    return str;
}
string add(string str,int x){
    int carry=x;
    for (int i=str.size()-1;i>=0;i--){
        int current=str[i]-'0'+carry;
        str[i]=current%10+'0';
        carry/=10;
    }
    if (carry) str='1'+str;
    return str;
}
int main(){
    string s;
    cin >> s;
    cout << divide(s,2);
}
```
- 辗转相除法求最大公约数，**两数的最小公倍数是它们的乘积除以最大公约数**，[最简真分数个数](https://www.nowcoder.com/questionTerminal/1f1db273eeb745c6ac83e91ff14d2ec9)
```cpp
int gcd(int a,int b){
	if (!b) return a;
	return gcd(b,a%b);
}
```
- [1081 Rational Sum](https://pintia.cn/problem-sets/994805342720868352/exam/problems/994805386161274880)，测试点4错误，最后[参考PAT甲级1081 Rational Sum (20分)记录](https://blog.csdn.net/weixin_41359213/article/details/113421356)（学习的点：scanf 控制格式化输入x.x，%lld），下面是自己的做法
```cpp
#include <bits/stdc++.h>
using namespace std;
typedef long long ll;
ll gcd(ll a,ll b){
    if (!b) return a;
    return gcd(b,a%b);
}
ll mcm(ll a,ll b){
    ll c=a*b;
    ll r=c/gcd(a,b);
    return r;
}

int main(){
    //求最大公约数
    int n;scanf("%d",&n);
    //每次找到已有分数分母的最小公倍数
    ll un,up,tmp_un,tmp_up,r_un,r_up,gc;
    scanf("%lld/%lld",&up,&un);
    //cout << un << ' ' << up;
    for (int i=1;i<n;i++){
        scanf("%lld/%lld",&tmp_up,&tmp_un);
        if (un==0){
            printf("0");
            return 0;
        }
        if (tmp_un==0){
            printf("0");
            return 0;
        }
        r_un=mcm(un,tmp_un);
        if (r_un==0){
            printf("0");
            return 0;
        }
        r_up=(r_un/un)*up+(r_un/tmp_un)*tmp_up;
        //cout << r_un << ' ' << r_up;
        //break;
        gc=gcd(r_un,r_up);
        if (gc==0){
            printf("0");
            return 0;
        }
        un=r_un/gc;
        up=r_up/gc;
    }
    //for (auto x:denominator) cout << x << ' ';
    bool flag=false;
    if (up<0) cout << '-';
    if (up/un){
        flag=true;
        cout << up/un;
    }
    if (up%un){
        if (flag) cout << ' ';
        cout <<up%un << '/' << un;
    }

}
```
- [第k个素数](https://www.nowcoder.com/questionTerminal/c5f8688cea8a4a9a88edbd67d1358415)，可以判断素数，也可以素数筛法，素数筛也可以辅助计算质因数分解，[\[编程题\]整除问题](https://www.nowcoder.com/questionTerminal/8e29045de1c84d349b43fdb123ab586a) [整除问题【质因子分解】](https://blog.csdn.net/csyifanZhang/article/details/105754286)
```cpp
#include<bits/stdc++.h>
using namespace std;

bool isPrime(int x){
    if (x<2) return false;
    for (int i=2,len=int(sqrt(x));i<=len;i++){
        if (x%i==0) return false;
    }
    return true;
}

int upBound=1e2;
vector<int> prime;
vector<bool> isPrimeTable(upBound,true);
void init(){
    isPrimeTable[0]=false;
    isPrimeTable[1]=false;
    for (int i=2;i<upBound;i++){
        if (!isPrimeTable[i]) continue;
        //不是素数就跳过,是素数记录,然后标记倍数为非素数
        prime.push_back(i);
        for (int j=i*i;j<upBound;j+=i){
            //i*i之前的数字已经被标记过了,这里是易忽略的点
            isPrimeTable[j]=false;
        }
    }
}


int main(){
    init();
    for (auto x:prime) cout << x << ' ';
    cout << endl;
    for (int i=0;i<upBound;i++){
        if (isPrime(i)) cout << i << ' ';
    }
}

int main(){
    //计算n的质因数分解
    int n=98;
    init();
    map<int,int> m; //key是质因数,value是此质因数的指数
    for (int i=0,len=prime.size();i<len && prime[i]<=n;i++){
        //试除
        int factor=prime[i];
        while (n%factor==0){
            n/=factor;
            m[factor]++;
        }
    }
    for (auto& x:m){
        cout << x.first << ' ' << x.second << endl;
    }
}
```
- 计算约数的个数，可以直接用质因数分解计算，也可以除法试
![题目](analysis.png)

```cpp
//计算约数个数 https://baike.baidu.com/item/%E7%BA%A6%E6%95%B0/8417882?fr=aladdin
//i*i<num的形式，数值稳定性更好
#include<iostream>
using namespace std;
int numOfDivisor(int num)
{
	int ans = 0;
	int i;
	for (i = 1; i*i<num; i++)
	{
		if (num%i == 0)
			ans += 2;
	}
	if (i*i == num) ans++;
	return ans;
}
int main()
{
	int n, num;
	while (cin >> n)
	{
		for (int i = 0; i<n; i++)
		{
			cin >> num;
			cout << numOfDivisor(num) << endl;
		}
	}
	return 0;
}
```
- 利用二进制表示快速幂，矩阵快速幂将 answer 初始化为单位阵，数字快速幂初始化为1
```cpp
#include <bits/stdc++.h>
using namespace std;

int fastExp(int a,int b,int mod){
    //求a^b的后三位数字
    //由于会溢出,快速幂的时候只需每次保留后几位即可
    //将b分解为二进制数,如果某位为1,答案就乘a^k次
    int answer=1;
    while (b){
        if (b%2){
            //b的二进制数此位为1
            answer*=a;
            //只保留后几位,保留后三位mod=1000
            answer%=mod;
        }
        b/=2;
        a*=a;
        a%=mod;
    }
    return answer;
}

int main(){
    int a=2,b=9;
    printf("%d",fastExp(a,b,10000));
}

#include <bits/stdc++.h>
using namespace std;

class sMatrix{
public:
    vector<vector<int>> matrix; //n*n行的矩阵
public:
    sMatrix(int n){
        matrix.resize(n);
        for (int i=0;i<n;i++) matrix[i].resize(n);
    }

    sMatrix operator*(const sMatrix& y)const{
        //两个方阵相乘
        sMatrix answer(matrix.size());
        for (int i=0,row=matrix.size();i<row;i++){
            for (int j=0,col=y.matrix[0].size();j<col;j++){
                answer.matrix[i][j]=0;
                for (int k=0,xcol=matrix[0].size();k<xcol;k++){
                    answer.matrix[i][j]+=matrix[i][k]*y.matrix[k][j];
                }
            }
        }
        return answer;
    }
    sMatrix fastExp(int k){
        sMatrix answer(matrix.size());
        //初始化为单位矩阵
        for (int i=0,row=answer.matrix.size();i<row;i++){
            for (int j=0,col=answer.matrix[0].size();j<col;j++){
                if (i==j) answer.matrix[i][j]=1;
                else answer.matrix[i][j]=0;
            }
        }
        sMatrix x=*this;
        while (k!=0){
            if (k%2) answer=answer*x;
            k/=2;
            x=x*x;
        }
        return answer;
    }
};

ostream& operator<<(ostream& out,const sMatrix& x){
    for (int i=0,row=x.matrix.size();i<row;i++){
        for (int j=0,col=x.matrix[0].size();j<col;j++){
            printf("%d ",x.matrix[i][j]);
        }
        printf("\n");
    }
    return out;
}


int main(){
    int n=2;
    sMatrix x(n);
    for (int i=0,row=x.matrix.size();i<row;i++){
        for (int j=0,col=x.matrix[0].size();j<col;j++){
            scanf("%d",&x.matrix[i][j]);
        }
    }
    cout << x.fastExp(2);
}

```
# 5.模拟输出类
- 输出时记得考虑实际情况（常常在边界出问题），如买东西不能买负数个
- 两个 int 相除得到的一定是 int，可能会丢失小数部分导致出错
- 输出给定图形的时候如果规律顺序按照从上到下，从左到右，与输出顺序一致时，可以直接输出，比如【按行输出梯形】，否则可以先构造图形再输出，比如【构造螺旋】
- 构造图形的时候以找好**锚定点**即可，以锚定点开始进行+-操作，比如方形的输出就可以抓住左上角顶点和右下角顶点的坐标作为锚定点
```cpp
//从外向内构造边长为n的螺旋
char matrix[80][80];
for (int i=0;i<=n/2;i++){    //(i,i)是每圈左上角坐标
    int j=n-1-i;            //(j,j)是每圈右下角坐标
    int len=n-2*i;
    char c;    //填充字符
    for (int k=0;k<len;k++){
        matrix=[i][i+k]=c;
        matrix=[i+k][i]=c;
        matrix=[j][j-k]=c;
        matrix=[j-k][j]=c;
    }
}
```
[Repater](https://www.nowcoder.com/questionTerminal/97fd3a67eff4455ea3f4d179d6467de9?f=discussion) **抓住图案的端点值和边长即可**，注意其他地方要填充空格，不能没有东西，之前直接 clear 是不行的，下面以 bb 新创建一个空格模板成功AC（搞到凌晨3点TvT
```cpp
#include <bits/stdc++.h>
using namespace std;
vector<vector<char>> cur;
vector<vector<char>> pre;
vector<vector<char>> basic;
void print(vector<vector<char>>& v){
    for (int i=0,len=v.size();i<len;i++){
        for (int j=0;j<len;j++) printf("%c",v[i][j]);
        printf("\n");
    }
}
void copyFromAnother(vector<vector<char>>& a,vector<vector<char>>& b){
    //以a为基础元素,填充到b中
    //对基本模板中每一个非空格元素替换为a整个图案
    int alen=a.size();
    int blen=alen*basic.size();
    //for (int i=0,len=b.size();i<len;i++) b[i].clear();
    vector<vector<char>> bb(blen,vector<char>(blen,' '));
    b=bb;
    /*
    printf("---------------b is --------------\n");
    print(b);
    printf("---------------b is --------------\n");
    */
    for (int i=0,len=basic.size();i<len;i++){
        for (int j=0;j<len;j++){
            if (basic[i][j]!=' '){
                //在cur中的左上角位置是i*len,j*len;
                int r=i*alen,c=j*alen;
                for (int ri=0;ri<alen;ri++){
                    for (int ci=0;ci<alen;ci++){
                        //拷贝b的整个图案
                        b[r+ri][c+ci]=a[ri][ci];
                        //cout << r+ri << ' ' << c+ci << endl;
                    }
                }
            }

        }
    }

}
int main(){
    int n,m;
    char c;
    n = 1;
    while (n){
        cin >> n;
        cur.clear();
        basic.clear();
        pre.clear();
        //输出一轮basic更新一次,以更新后的basic再输出
        if (n==0) return 0;
        getchar();
        pre.resize(n);
        basic.resize(n);
        for (int i=0;i<n;i++){
            for (int j=0;j<n;j++){
                c=getchar();
                pre[i].push_back(c);
                basic[i].push_back(c);
            }
            getchar();
        }
        scanf("%d",&m);
        if (m==1){
            print(pre);
            continue;
        }
        for (int i=1;i<m;i++){
            //以上一次的basic为基础保存在cur,上次用完要清除
            if (i%2) copyFromAnother(pre,cur);  //pre->cur
            else copyFromAnother(cur,pre);      //cur->pre
            /*
            printf("-i=%d----------pre is --------------\n",i);
            print(pre);
            printf("---------------pre is --------------\n",i);
            printf("-i=%d----------cur is --------------\n",i);
            print(cur);
            printf("---------------cur is --------------\n",i);
            */
        }
        //m是偶数,结果存储在cur,奇数存储在pre
        if (m%2) print(pre);
        else print(cur);
        //printf("%d %d",pre.size(),cur.size());
    }
    return 0;
}
```
- 向文档写入
```cpp
#include <iostream>
#include <fstream>
using namespace std;

int main(){
	ofstream dataFile;
	dataFile.open("C:\\Users\\PC\\Destop\\t.txt", ofstream::app);
	fstream file("C:\\Users\\PC\\Destop\\t.txt", ios::out);
	dataFile << a[1][1] << ' '<< b << endl;     // 写入数据
	dataFile.close();                           // 关闭文档
}
```
- 计算日期注意闰年判断和天数提前保存，[日期差值](https://www.nowcoder.com/questionTerminal/ccb7383c76fc48d2bbc27a2a6319631c)，计算星期几需要计算与1年1月1日（星期一）的差值，再对7求余
```cpp
#include <bits/stdc++.h>
using namespace std;
bool isLeapYear(int year){
    //注意闰年的判断方式
    return (year%4==0 && year%100!=0) || (year%400==0);
}
int main(){
    string s1,s2;
    int date[2][13]={    //注意前面有个0
        {0,31,28,31,30,31,30,31,31,30,31,30,31},
        {0,31,29,31,30,31,30,31,31,30,31,30,31}    //闰年
    };
    while (cin >> s1){
        cin >> s2;
        //总保证s1<s2
        if (s1>s2) swap(s1,s2);
        //cout << s1 << ' ' << s2;
        int year1=stoi(s1.substr(0,4));
        int month1=stoi(s1.substr(4,2));
        int day1=stoi(s1.substr(6,2));
        int year2=stoi(s2.substr(0,4));
        //cout << year1 << ' ' << month1 << ' ' << day1;
        int month2=stoi(s2.substr(4,2));
        int day2=stoi(s2.substr(6,2));;
        int dd1=0,dd2=0;
        //计算年之间的差距
        //将它们都转换成year1年开始过的天数
        int row1=0;
        if (isLeapYear(year1)) row1=1;
        for (int month=1;month<month1;month++){
            dd1+=date[row1][month];
        }
        dd1+=day1;
        for (int year=year2;year>year1;year--){
            if (isLeapYear(year)) dd2+=366;
            else dd2+=365;
        }
        for (int month=1;month<month2;month++){
            dd2+=date[row1][month];
        }
        dd2+=day2;
        printf("%d",dd2-dd1+1);
    }
}
```
- [【最详细的分析】1061 Dating (20 分)](https://blog.csdn.net/weixin_43899069/article/details/114156711?ops_request_misc=%257B%2522request%255Fid%2522%253A%2522168044879816800182731301%2522%252C%2522scm%2522%253A%252220140713.130102334..%2522%257D&request_id=168044879816800182731301&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~sobaiduend~default-1-114156711-null-null.142%5Ev80%5Einsert_down38,201%5Ev4%5Eadd_ask,239%5Ev2%5Einsert_chatgpt&utm_term=1061%20dating%20%E6%B5%8B%E8%AF%95%E7%82%B91.2.4&spm=1018.2226.3001.4187)  注意隐含条件
- [\[编程题\]路径打印](https://www.nowcoder.com/questionTerminal/64b472c9bed247b586859978d13145ad?f=discussion)，格式错误，未解之谜（气死了
```cpp
#include <bits/stdc++.h>
using namespace std;
char last;   //最后一个打印的字符
class tree{
public:
    //多叉树
    string name;
    vector<tree*> children;
    tree(string n):name(n){};
};
tree* expand(tree* cur,const string& name){
    //如果name不在cur的子列表中就增添一项,然后返回新创建的节点
    //如果在子列表就返回这个节点
    for (int i=0,len=cur->children.size();i<len;i++){
        if (cur->children[i]->name==name) return cur->children[i];
    }
    //创建新节点返回
    tree* child=new tree(name);
    cur->children.push_back(child);
    return child;
}
bool cmp(const tree* t1,const tree* t2){
    return t1->name<t2->name;
}
void adjust(tree* cur){
    sort(cur->children.begin(),cur->children.end(),cmp);
    for (int i=0,len=cur->children.size();i<len;i++){
        adjust(cur->children[i]);
    }
}
void print(tree* cur,int cnt){
    //cnt是空格数量
    //输出cnt个空格
    for (int i=0;i<cnt;i++){
        printf(" ");
    }
    last=' ';
    cout << cur->name;//cout << cur->children.size(); //后面没孩子,也没重新到根目录下,为什么输出了S？？？
    last=cur->name.back();
    //cout << cnt;
    //这个目录后面还有输出并且不是递归结束才打印,递归结束的话外面会打
    if (cur->children.size()!=0){
        printf("\n");
        last='\n';
    }
    for (int i=0,len=cur->children.size();i<len;i++){
        print(cur->children[i],cnt+2);
    }
}
int main(){
    int n;
    while (scanf("%d",&n)){
        if (n==0) return 0;
        string s;
        tree* root=new tree("root");
        tree* cur;
        getchar();
        for (int i=0;i<n;i++){
            s="";
            cur=root;
            char c='x';
            while (c!='\n'){
                c=getchar();
                if (c=='\\'){
                    //cout << "s=" << s << endl;
                    cur=expand(cur,s);
                    s="";
                }
                else s+=c;
            }
            //根据最后一个字符是不是/来判断要不要处理最后一个字符
            //cout << "s=" << s << endl;
            if (s!="\n" && s!="\\") cur=expand(cur,s);
        }
        //排序
        adjust(root);
        //输出
        for (int i=0,len=root->children.size();i<len;i++){
            print(root->children[i],0);
            if (last!='\n') printf("\n");
            //printf("结束");
       }
        printf("\n");
        last='\n';
    }
}
```
- [\[编程题\]约瑟夫问题II](https://www.nowcoder.com/questionTerminal/ff063da83b1a4d91913dd1b1e8b01466)，涉及到大量删除操作用链表解决更好，下面要注意的是 vector 边删除边遍历的操作，如果 for 里也 it++ 会跳过某些元素
```cpp
class Joseph {
public:
    int getResult(int n) {
        // write code here
        int turn=2,callIndex;
        vector<int> v;
        for (int i=1;i<=n;i++) v.push_back(i);
        vector<int> call;call.push_back(1);
        //没报1的都出局
        while (v.size()>1){
            call.push_back(turn++);
            callIndex=0;
            for (auto it=v.begin();it!=v.end();){
                if (call[(callIndex++)%call.size()]!=1) it=v.erase(it);
                else it++;
            }
            //把最后一个元素挪到第一个
            v.insert(v.begin(),v.back());
            v.erase(--v.end());
        }
        return v[0];
    }
};
};
```
- [1046 Shortest Distance](https://pintia.cn/problem-sets/994805342720868352/exam/problems/994805435700199424)，第一次做简单相加会超时，第二次做用二维数组存储累积距离会超限，可以直接用一维数组解决，只记录起点到其他点的累积距离
```cpp
#include <bits/stdc++.h>
using namespace std;
const int inf=1e7;
int main(){
    int n,m,v1,v2;scanf("%d",&n);
    vector<vector<int>> v(n,vector<int>(n,0));
    int sum=0,k;
    for (int i=0;i<n;i++){
        //从i->(i+1)%n的距离是k
        scanf("%d",&k);
        sum+=k;
        v[i][(i+1)%n]=k;
        for (int j=0;j<i+1;j++){
            v[j][(i+1)%n]=v[j][i]+k;
        }
    }
    /*
    for (auto x:v){
        for (auto y:x){
            cout << y << ' ';
        }
        cout << endl;
    }
    cout << sum << endl;
    */
    scanf("%d",&m);
    for (int i=0;i<m;i++){
        scanf("%d%d",&v1,&v2);
        int res=v[v1-1][v2-1];
        res=sum-res>res?res:sum-res;
        cout << res << endl;
    }
}

#include <bits/stdc++.h>
using namespace std;
int main(){
    int n,m,v1,v2;scanf("%d",&n);
    vector<int> v(n+1,0); //从0->i的距离是v[i]
    int sum=0,k;
    for (int i=1;i<=n;i++){
        scanf("%d",&k);
        v[i]=v[i-1]+k;
        sum+=k;
    }
    //for (auto x:v) cout << x << endl;
    scanf("%d",&m);
    for (int i=0;i<m;i++){
        scanf("%d%d",&v1,&v2);
        int res=v[v2-1]-v[v1-1];
        res=res>0?res:-res;
        //cout << res << endl;
        res=sum-res>res?res:sum-res;
        printf("%d\n",res);
    }
}
```
# 6.其他
- 万能头文件 #include <bits/stdc++.h>
- [关于位运算的一些技巧](https://zhuanlan.zhihu.com/p/564808922)
- 贪心思想
  - [代理服务器](https://www.nowcoder.com/questionTerminal/1284469ee94a4762848816a42281a9e0)，实际是模拟页面置换算法OPT
```cpp
while(begin != m){
    for (i=0; i<n; i++){
        //遍历服务器ip[i],找到能够到达的最远处
        for (j=begin; j<m; j++){
            if (ip[i] == server[j])
                break;
        }
        //该循环结束后，如果j==m,则说明走通，否则表明最远可走到server[j]
        if (j>max)
            max = j;
    }
    begin = max;
    //找到并记录所有***ip中最远能走到的server，下次从这里开始搜索
    count++;
    //无论如何count都会大于等于1，所以最后输出时要-1
        }
```
- [今年暑假不AC，贪心思想，按照最先结束选择区间](https://blog.csdn.net/qq_44622401/article/details/103439954?ops_request_misc=&request_id=&biz_id=102&utm_term=%E4%BB%8A%E5%B9%B4%E6%9A%91%E5%81%87%E4%B8%8Dac&utm_medium=distribute.pc_search_result.none-task-blog-2~all~sobaiduweb~default-3-103439954.142%5Ev68%5Econtrol,201%5Ev4%5Eadd_ask,213%5Ev2%5Et3_control2&spm=1018.2226.3001.4187)
- [贪心算法题目](https://programmercarl.com/%E8%B4%AA%E5%BF%83%E7%AE%97%E6%B3%95%E7%90%86%E8%AE%BA%E5%9F%BA%E7%A1%80.html)
- [Catch That Cow](https://blog.csdn.net/qq_43984169/article/details/86745862?ops_request_misc=%257B%2522request%255Fid%2522%253A%2522167229987516782429760718%2522%252C%2522scm%2522%253A%252220140713.130102334..%2522%257D&request_id=167229987516782429760718&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~top_positive~default-1-86745862-null-null.142%5Ev68%5Econtrol,201%5Ev4%5Eadd_ask,213%5Ev2%5Et3_control2&utm_term=catch%20that%20cow&spm=1018.2226.3001.4187)，[Find The Multiple](https://blog.csdn.net/Nacht_one/article/details/81745096?ops_request_misc=%257B%2522request%255Fid%2522%253A%2522167230054016800184144303%2522%252C%2522scm%2522%253A%252220140713.130102334..%2522%257D&request_id=167230054016800184144303&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~sobaiduend~default-4-81745096-null-null.142%5Ev68%5Econtrol,201%5Ev4%5Eadd_ask,213%5Ev2%5Et3_control2&utm_term=find%20the%20multiple&spm=1018.2226.3001.4187) ，[\[编程题\]玛雅人的密码](https://www.nowcoder.com/questionTerminal/761fc1e2f03742c2aa929c19ba96dbb0) 宽度优先搜索
- [A Knight's Journey](https://blog.csdn.net/qq_37164003/article/details/77841342?ops_request_misc=&request_id=&biz_id=102&utm_term=a%20knights%20journey&utm_medium=distribute.pc_search_result.none-task-blog-2~all~sobaiduweb~default-7-77841342.nonecase&spm=1018.2226.3001.4187)，[square](https://blog.csdn.net/weixin_42114926/article/details/105563267?ops_request_misc=&request_id=&biz_id=102&utm_term=square%20%E6%B7%B1%E5%BA%A6%E4%BC%98%E5%85%88%E6%90%9C%E7%B4%A2&utm_medium=distribute.pc_search_result.none-task-blog-2~all~sobaiduweb~default-1-105563267.nonecase&spm=1018.2226.3001.4187) 深度优先搜索
- [1041 Be Unique](https://pintia.cn/problem-sets/994805342720868352/exam/problems/994805444361437184)，一个vector记录顺序，一个map统计数字，即可既根据输入顺序也根据统计情况做判断
- [1040 Longest Symmetric String](https://pintia.cn/problem-sets/994805342720868352/exam/problems/994805446102073344)，自己做的只看到了例子的那种情况，对称分两种对称情况，[【PAT甲级 - C++题解】1040 Longest Symmetric String](https://blog.csdn.net/Newin2020/article/details/128691669)
```cpp
#include <bits/stdc++.h>
using namespace std;

int main(){
    string s;
    getline(cin,s);
    //cout << s;
    int res=0;
    //遍历对称中心
    for (int i=0,len=s.size();i<len;i++){
        int center=i;
        int curlen=1;
        int offest=1;
        while (center-offest>=0 && center+offest<len){
            if (s[center-offest]==s[center+offest]){
                offest++;
                curlen+=2;
            }
            else break;
        }
        //cout << s[center] << ' ' << curlen << endl;
        if (curlen>res) res=curlen;
    }
    printf("%d",res);
}
```
- [1037 Magic Coupon](https://editor.csdn.net/md/?articleId=127224482#2_30)
```cpp
#include <bits/stdc++.h>
using namespace std;
bool cmp(int a,int b){
    return abs(a)>abs(b);
}

int main(){
    int sum=0;
    int nc,np,tmp;
    scanf("%d",&nc);
    vector<int> Ncoupon;
    vector<int> Pcoupon;

    for (int i=0;i<nc;i++){
        scanf("%d",&tmp);
        if (tmp<0) Ncoupon.push_back(tmp);
        else Pcoupon.push_back(tmp);
    }
    sort(Ncoupon.begin(),Ncoupon.end(),cmp);
    sort(Pcoupon.begin(),Pcoupon.end(),cmp);

    scanf("%d",&np);
    vector<int> product(np);
    for (int i=0;i<np;i++) scanf("%d",&product[i]);
    sort(product.begin(),product.end(),cmp);
    int pi=0,ni=0;
    for (int i=0;i<np;i++){
        //cout << product[i] << endl;
        if (product[i]<0){
            if (ni<Ncoupon.size()) sum+=product[i]*Ncoupon[ni++];
            else sum+=0;
        }
        else{
            if (pi<Pcoupon.size()) sum+=product[i]*Pcoupon[pi++];
            else sum+=product[i];
        }
    }
    printf("%d",sum);
}
```
- [1056 Mice and Rice](https://pintia.cn/problem-sets/994805342720868352/exam/problems/994805419468242944) 遍历模拟比赛，需要原地修改成员变量的可以用指针vector，[解答](https://blog.csdn.net/qq_39837806/article/details/104450363?ops_request_misc=%257B%2522request%255Fid%2522%253A%2522167920961216800225567492%2522%252C%2522scm%2522%253A%252220140713.130102334.pc%255Fall.%2522%257D&request_id=167920961216800225567492&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~first_rank_ecpm_v1~rank_v31_ecpm-8-104450363-null-null.142%5Ev74%5Einsert_down4,201%5Ev4%5Eadd_ask,239%5Ev2%5Einsert_chatgpt&utm_term=1056%20Mice%20and%20Rice&spm=1018.2226.3001.4187)

