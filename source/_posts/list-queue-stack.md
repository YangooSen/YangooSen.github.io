---
title: list_queue_stack
katex: true
date: 2024-04-22 21:19:22
excerpt: 链表，队列，堆栈和应用题目
tag: ds
---

[1051 Pop Sequence](https://pintia.cn/problem-sets/994805342720868352/exam/problems/994805427332562944)
```cpp
#include <bits/stdc++.h>
using namespace std;
stack<int> s;
int n,m,k;
int tmp;
int main(){
    scanf("%d%d%d",&m,&n,&k);
    for (int i=0;i<k;i++){
        stack<int> s;
        int cnt=1;
        bool flag=false;
        for (int i=0;i<n;i++){
            scanf("%d",&tmp);
            if (!s.empty() && s.top()==tmp){
                //cout << "debug1" << endl;
                s.pop();
            }
            else if (s.size()<m){
                //cout << "debug2" << endl;
                while (cnt!=tmp && s.size()<m){
                    s.push(cnt);
                    cnt++;
                }
                //cout << s.size() << ' ' << tmp << endl;
                if (s.size()==m){
                    //没找到这个数字就满了
                    flag=true;
                }
                //cnt==tmp相等的时候不压入
                else cnt++;
            }
            else{
                //cout << "debug3" << endl;
                flag=true;
            }
        }
        if (!flag) printf("YES\n");
        else printf("NO\n");
    }

}
```
[7-22 堆栈模拟队列](https://pintia.cn/problem-sets/15/problems/837)，能模拟的队列容量 <= 两个堆栈容量之和，可以参考[232.用栈实现队列](https://programmercarl.com/0232.%E7%94%A8%E6%A0%88%E5%AE%9E%E7%8E%B0%E9%98%9F%E5%88%97.html#%E6%80%9D%E8%B7%AF)中的图，主要想法是**通过两个栈的倒换改变元素的顺序**，而[225. 用队列实现栈](https://programmercarl.com/0225.%E7%94%A8%E9%98%9F%E5%88%97%E5%AE%9E%E7%8E%B0%E6%A0%88.html)中可以**通过先入队再出队的方法备份前面的元素**。[猫狗收养问题是典型的队列应用](https://www.cnblogs.com/juanzhi/p/12720485.html)
- [1032 Sharing](https://pintia.cn/problem-sets/994805342720868352/exam/problems/994805460652113920)从后往前遍历会有测试点不通过。[解答](https://www.liuchuo.net/archives/2113)
```cpp
#include <stack>
#include <iostream>
using namespace std;

class myQ{
public:
    stack<int> sin;
    stack<int> sout;
    int inSize,outSize,inCap,outCap;
    //必须小的那个栈做输入栈,大的栈做输出栈,因为输入栈满了之后要倒入输出栈
    myQ(int max,int min):inCap(min),outCap(max),inSize(0),outSize(0){};
    void AddQ(int x){
        //输入的元素放入in,如果满了就看看能不能放到sout
        if (inSize<inCap){
            sin.push(x);
            inSize++;
        }
        else{
            //inSize==inCap
            if (!sout.empty()){ //这里不能用outCap==outSize判断
                cout << "ERROR:Full" << endl;
                return;
            }
            //倒入输入栈
            while (!sin.empty()){
                int x=sin.top();sin.pop();
                sout.push(x);
                outSize++;
            }
            sin.push(x);
            inSize=1;
        }
    }
    void DeleteQ(){
        int x;
        //输出输出栈的内容,如果没有的话就看看输入栈
        if (!sout.empty()){
            x=sout.top();sout.pop();
            outSize--;
        }
        else{
            if (sin.empty()){
                cout << "ERROR:Empty" << endl;
                return;
            }
            while (!sin.empty()){
                int x=sin.top();sin.pop();
                sout.push(x);
                outSize++;
            }
            inSize=0;
            x=sout.top();sout.pop();
            outSize--;
        }
        cout << x << endl;
    }
};

int main(){
    int n1,n2;
    myQ* q;
    cin >> n1 >> n2;
    if (n1>n2) q=new myQ(n1,n2);
    else q=new myQ(n2,n1);
    int x;
    char c;
    cin >> c;
    while (c!='T'){
        if (c=='A'){
            cin >> x;
            q->AddQ(x);
        }
        else if (c=='D') q->DeleteQ();
        //cout << "inSize=" << q->inSize << "outSize=" << q->outSize << endl;
        cin >> c;
    }
}
```
下面是[中缀表达式转后缀表达式](https://pintia.cn/problem-sets/15/exam/problems/827)，后缀表达式求值直接用堆栈可解决，前缀表达式求值只需读入全部字符串，再倒着遍历字符，处理方法与后缀表达式求值类似。[后缀表达式求值](https://leetcode.cn/problems/8Zf90G/)，[前缀表达式求值](https://pintia.cn/problem-sets/15/exam/problems/836)一定要熟练，[视频在这](https://www.bilibili.com/video/BV1Z7411R7eC?p=20&vd_source=6c26f427606a59575440e9bc6cec44af)，**中缀表达式求值可以先写出下面的转换，再利用堆栈计算转换后的后缀表达式**
```cpp
//数字的相对顺序不变,计算符号的相对顺序改变
//存储"等待中"的运算符号,将当前运算符号与"等待中"的最后一个运算符号比较
//如果当前运算符优先级低,那么栈内优先级高的就可以输出
//左括号在外面优先级最高,在栈内优先级最低
//遇到右括号,输出到左括号结束
//同一优先级堆栈内的先输出

/*
测试点

序号    输入                     输出                                   说明
0    2+3*(7-4)+8/4            2 3 7 4 - * + 8 4 / +            正常测试6种运算符
1    ((2+3)*4-(8+2))/5        2 3 + 4 * 8 2 + - 5 /            嵌套括号
2    1314+25.5*12             1314 25.5 12 * +                 运算数超过1位整数且有非整数出现
3    -2*(+3)                  -2 3 *                           运算数前有正负号
4    123                      123                              只有一个数字

*/

#include <stack>
#include <map>
#include <vector>
#include <algorithm>
#include <iostream>
using namespace std;

int main(){
    //m是计算符的优先级,( 放入堆栈内优先级最低
    map<char,int> m={{'(',0},{')',0},{'+',1},{'-',1},{'*',2},{'/',2}};
    string str;
    cin >> str;
    stack<char> s;
    string num; //存储数字
    vector<string> res; //存储输出结果
    for (int i=0,len=str.size();i<len;i++){
        //下面第一个if是得到数字,这个是细节最多的地方
        if(isdigit(str[i]) || (((i==0 || str[i-1]=='(')) && (m[str[i]]==1))){
            //单纯数字                 //({正负号}{数字})的情况,即序号3的输入
            if(str[i]!='+') num={str[i]};
            while(isdigit(str[i+1]) || str[i+1]=='.'){
                num+=str[i+1];
                i++;
            }
            res.push_back(num);
            num="";
        }
        else{
            if (str[i]=='(') s.push(str[i]);
            else if (str[i]==')'){
                while (!s.empty() && s.top()!='('){
                    res.push_back({s.top()});s.pop();
                }
                s.pop();
            }
			else{
				//						注意这里是<=
				while(!s.empty()&&m[str[i]]<=m[s.top()]){//这里需要注意，如果是空栈，直接压栈即可，如果栈顶元素的优先级大于等于s【i】，需要先将栈顶元素放入v中，再进行压栈，这里的循环是重点，需要掌握。
					res.push_back({s.top()});s.pop();
				}
				s.push(str[i]);
            }
        }
    }
    while (!s.empty()){
        res.push_back({s.top()});s.pop();
    }
    cout << res[0];
    for (int i=1,len=res.size();i<len;i++) cout << ' ' << res[i];
    return 0;
}

```

```cpp
#include <iostream>
#define MAXSIZE 10000
using namespace std;

template<typename T>
class LNode{ //顺序实现
    public:
        T data[MAXSIZE];
        int last; //最后一个元素的下标
    LNode():last(-1){}; //这里必须是-1,添加一个元素后是0,满足insert
    int find(T x){
        int i=0;
        while (i<=last && data[i]!=x) x++;
        if (i>last) return -1;
        return i;
    }
    void insert(int i,T x){
        int j;
        // 注意下面的两个判定
        if (last==MAXSIZE-1){
            cout << "表满" << endl;
            return;
        }
        if (i<0 || i>last+1){ //可以插到last+1,不能插到last+2
            cout << "位置不合法" << endl;
            return;
        }
        for (j=last;j>=i;j--) data[j+1]=data[j]; //注意遍历顺序及等号的有无
        data[i]=x;
        last++;
    }
    void Delete(int i){
        if (i<0 || i>last){
            cout << "元素不存在" << endl;
            return;
        }
        for (int j=i;j<last;j++){ //注意遍历顺序
            data[j]=data[j+1];
        }
        last--;

    }
};
template<typename T>
class lNode{ //链表实现
    public:
    T data;
    lNode* next;
    lNode(T x):data(x),next(nullptr){};
    int length(){
        int i=1;
        lNode* head=this;
        while (head->next){
            i++;
            head=head->next;
        }
        return i;
    }
    lNode* insert(int i,T x){
        lNode* s=new lNode(x);
        if (i==0){
            //要分头部和非头部
            s->next=this;
            return s;
        }
        lNode* p=findKth(i-1); //△插入删除都是找i-1个元素△
        if (!p){
            cout << "参数错误" << endl;
            return this;
        }
        s->next=p->next;
        p->next=s;
        return this;
    }
    lNode* findX(T x){
        lNode* head=this;
        while (head && head->data!=x){
            head=head->next;
        }
        return head; //不需要if判断,空了自然就返回null
    }
    lNode* findKth(int k){
        int i=0;
        lNode* head=this;
        while (head && i<k){
            head=head->next;
            i++;
        }
        if (i==k) return head;
        return nullptr;
    }
    lNode* deleteKth(int k){
        if (k==0){
            if (this==nullptr){
                cout << "头节点为空" << endl;
                return nullptr;
            }
            else{
                lNode* head=this;
                lNode* s=head->next;
                delete head;
                return s;
            }
        }
        lNode* p=findKth(k-1);
        if (!p){
            cout << k-1 << " 节点不存在" << endl;
            return this;
        }
        if (!p->next){
            cout << k << " 节点不存在" << endl;
            return this;
        }
        lNode* s=p->next; //要删除的是p->next
        p->next=s->next;
        delete s;
        return this;
    }


};
template<typename T>
class GNode{ //广义表,表中的元素是另一个广义表或单元素,可以用union
    int tag; //0表示单元素
    union{
        T data;
        GNode* subList;
    }UReign;
    GNode* next;
};

template<typename T>
class Stack{
    //后入先出,数组实现
    T data[MAXSIZE];
    int top;
    Stack():top(-1){};
    void push(T x){
        if (top==MAXSIZE-1){
            cout << "栈满" << endl;
            return;
        }
        data[++top]=x;
    }
    T pop(){
        if (top==-1){
            cout << "栈空" << endl;
            return nullptr;
        }
        return data[top--];
    }


};

template<typename T>
class DStack{
    //一个数组实现两个堆栈
    T data[MAXSIZE];
    int top1;
    int top2;
    DStack():top1(-1),top2(MAXSIZE){};
    void push(T x,int tag){
        //tag是放哪个堆
        if (top2-top1==1){
            //比如两个指针挨在一起放不下
            cout << "栈满" << endl;
            return;
        }
        if (tag==1) data[++top1]=x;
        else data[--top2]=x;
    }
    T pop(int tag){
        if (tag==1){
            if (top1==-1){
                cout << "栈空" << endl;
                return nullptr;
            }
            else return data[top1--];
        }
        else{
            if (top2==MAXSIZE){
                cout << "栈空" << endl;
                return nullptr;
            }
            else return data[top2++];
        }
    }
};

template<typename T>
class SNode{
    //△链表实现堆栈,top必须是头节点△
    T data;
    SNode* next;
    SNode():next(nullptr){}; //头节点没有信息,dummy节点
    SNode(int x):data(x),next(nullptr){};
    bool isEmpty(){
        return next==nullptr;
    }
    void push(T x){
        SNode* s=new SNode(x);
        s->next=this->next;
        this->next=s;
    }
    T pop(){
        if (this->next==nullptr){
            cout << "栈空" << endl;
            return nullptr;
        }
        else{
            SNode* s=this->next;
            this->next=s->next;
            T d=s->data;
            delete s;
            return d;
        }
    }
};

template<typename T>
class Queue{
    T data[MAXSIZE];
    int front,rear; //△front是第一个元素前一个索引,rear是最后一个索引△
    //顺环队列rear和front相等时无法判断队列是空还是满
    //不放满队列可以解决
    Queue():front(0),rear(0){};
    void add(T x){  //△下一个放的位置是(rear+1)%MAXSIZE△
        if ((rear+1)%MAXSIZE==front){
            cout << "队列满" << endl;
            return;
        }
        rear=(rear+1)%MAXSIZE;
        data[rear]=x;
    }
    T DeleteX(){
        if (front==rear){
            cout << "队列空" << endl;
            return nullptr;
        }
        front=(front+1)%MAXSIZE;
        return data[front];
    }
};
template<typename T>
class node{ //链表节点
    T data;
    node* next;
    node(T x):data(x),next(nullptr){};
};
template<typename T>
class QNode{
    //链表实现queue
    //front做删除操作,rear做删除操作,链表头节点删掉之后找不到下一个,因此不能做front
    //只能头节点做front,尾结点做rear
    //△实现插入删除注意判断队列是否为空,很重要△
    node<T>* rear;
    node<T>* front;
    QNode():rear(nullptr),front(nullptr){};
    bool isEmpty(){
        return front==nullptr;
    }

    void add(T x){
        node<T>* s=new node<T>(x);
        if (isEmpty()){
            rear=front=s;
            return;
        }
        else{
            rear->next=s;
            rear=s;
        }
        return;
    }
    T Delete(){
        if (isEmpty()){
            cout << "队列空" << endl;
            return nullptr;
        }
        node<T>* s=front;
        if (front==rear){
            //队列只有一个元素
            front=nullptr;
            rear=nullptr;
        }
        else front=s->next;
        T d=s->data;
        delete s;
        return d;
    }
};

int main(){

    //栈实现中缀表达式
    //https://www.bilibili.com/video/BV1JW411i731?p=20&spm_id_from=pageDriver&vd_source=6c26f427606a59575440e9bc6cec44af&t=192.9
    lNode<int>* l=new lNode<int>(0);
    //LNode<int>* l=new LNode<int>();
    cout << l->data << ' ' << l->length() << endl;
    l=l->insert(0,1);
    l=l->insert(0,2);
    cout << l->data << ' ' << l->length() << endl;
    //l->insert(0,1);
    //l->insert(0,2);
    l=l->insert(0,3);
    l=l->insert(0,4);
    l=l->insert(0,5);
    l=l->deleteKth(0);
    cout << l->data << ' ' << l->length() << endl;

}

```
