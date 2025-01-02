---
title: hashtable
katex: true
date: 2024-12-30 21:20:44
excerpt: 散列表
tag: ds
---

[7-48 银行排队问题之单窗口“夹塞”版](https://pintia.cn/problem-sets/15/exam/problems/895)，可参考 [7-48 银行排队问题之单窗口“夹塞”版 (30 分)](https://blog.csdn.net/qq_48508278/article/details/119685298?ops_request_misc=&request_id=&biz_id=102&utm_term=7-48%20%E9%93%B6%E8%A1%8C%E6%8E%92%E9%98%9F%E9%97%AE%E9%A2%98%E4%B9%8B%E5%8D%95%E7%AA%97%E5%8F%A3%E2%80%9C%E5%A4%B9%E5%A1%9E%E2%80%9D%E7%89%88&utm_medium=distribute.pc_search_result.none-task-blog-2~all~sobaiduweb~default-0-119685298.nonecase&spm=1018.2226.3001.4187)
[字串计算](https://blog.csdn.net/ab417789/article/details/102300979)，[开门人和关门人](https://www.nowcoder.com/questionTerminal/a4b37b53a44d454ab0834e1517983215) 用 map 解决即可，map 底层是红黑树，unordered_map 底层是哈希表，查找效率比 map 高
```cpp
#include <iostream>
#include <cmath>
using namespace std;

template<typename keyType,typename valueType>
class Map{
    /*
    散列表性能与三个因素有关:
    (1).散列函数是否均匀
    (2).处理冲突的方法
    (3).散列表的装填因子
    */
public:
    //动态查找问题
    //用树的话,字符串作为根节点,比较大小的时候比较复杂
    //查找的本质是已知对象找对象,散列表是以空间换时间
    //散列基本工作:1.根据对象找位置,2.解决冲突
    //装填因子数据量/空间
    keyType* indexTokey;
    valueType* data;
    int maxSize;
    int* cellTypes;   //存储状态,0表示空,1表示有已删除元素,2表示有正常元素

    Map(int tableSize){
        //找到第一个大于tableSize的素数
        maxSize=(tableSize%2)?tableSize+2:tableSize+1;  //从奇数开始
        int i;
        while (true){
            for (i=int(sqrt(maxSize));i>2;i--){
                if (!(maxSize%i)) break; //确定不是素数
            }
            if (i==2) break;  //确定已经找到了那个素数
            maxSize+=2; //下一个奇数
        }

        indexTokey=(keyType*)malloc(sizeof(keyType)*maxSize);
        data=(valueType*)malloc(sizeof(valueType)*maxSize);
        cellTypes=(int*)malloc(sizeof(int)*maxSize);
        for (int i=0;i<maxSize;i++) cellTypes[i]=0; //初始是空

    }
    int Hash(keyType key){
        //整型关键字
        //Hash(key)=a*key+b 直接定址法
        return key%maxSize;

        //数字分析法将可变的位作为散列地址,例如身份证号码
        //return (key[6]-'0')*(10^4)+(key[10]-'0')*(10^3)+(key[14]-'0')*(10^2)...
        //折叠法,把关键字分割成位数相同的几个部分,然后相加
        //平方取中法,取(key*key)的中间几个数字
        /*
        //可表示范围太小
        int sum=0;
        for (int i=0,len=key.size();i<len;i++) sum+=(key[i]);
        return sum%maxSize;

        //简单改进
        //return (key[0]*(27*27)+key[1]*27+key[2])%maxSize;

        int sum=0;
        for (int i=0,len=key.size();i<len;i++) sum=(sum<<5)+key[i];  //和视频中略有不同
        return sum%maxSize;
        */
    }
    //处理冲突:开放地址法,链地址法
    //hash(key)=(hash(key)+di)%masSize,线性探测di=i,平方探测di=+-i^2,双散列i*h(key)
    //ASLs成功平均查找长度,ASLu不成功平均查找长度
    //散列表大小是4k+3时平方探测保证可以扫描遍全表
    //装填因子在[0.5,0.85]比较好,装填因子过大时可以加倍扩大散列表,这个过程叫做"再散列"

    int _Find(keyType key){  //根据key找到index
        int initPos=Hash(key);
        //cout << initPos << endl;
        int newPos=initPos;
        int cnt=0; //冲突次数,用于计算di
        while (cellTypes[newPos]!=0 && indexTokey[newPos]!=key){ //被"删除"的元素也可以找得到
        //☆☆☆☆☆☆☆☆☆☆☆☆☆☆☆☆☆☆☆☆☆☆☆☆
        //☆☆☆☆☆☆☆☆☆☆☆☆☆☆☆☆☆☆☆☆☆☆☆☆
            if (++cnt%2){
                //冲突奇数次
                newPos=initPos+((cnt+1)/2)*((cnt+1)/2); //+i^2
                while (newPos>=maxSize) newPos-=maxSize; //求余
            }
            else{
                newPos=initPos-(cnt/2)*(cnt/2); //-i^2
                while (newPos<0) newPos+=maxSize; //负数对正数取余会得到负数的余数
            }
        }
        return newPos;  //如果是空位就返回空位了
    }
    valueType Find(keyType key){
        int index=_Find(key);
        if (cellTypes[index]!=2){
            cout << "key没有对应value ";
            return valueType(-1);
        }
        return data[index];
    }
    void Insert(keyType key,valueType value){
        int index=_Find(key);
        if (cellTypes[index]!=2){ //0是空,1是有删除的元素,都可以插入
            data[index]=value;
            cellTypes[index]=2;
            indexTokey[index]=key;
        }
    }
    void Delete(keyType key){
        int index=_Find(key);
        cellTypes[index]=1;
    }
};

template<typename T>
class node{
    T data;
    node* next;
    node():next(nullptr){};
    node(T d):data(d),next(nullptr){};
};

template<typename T>
class HashTable{  //把T类型的key映射为整型索引
    //分离链接法,装填因子可能超过1
    int maxSize;
    node<T>* data;

    NodeMap(int tableSize){
        data=(node<T>*)malloc(sizeof(node<T>)*tableSize);
        tableSize=maxSize;
    }
    int Hash(T key){
        return key%maxSize;
    }
    node<T>* Find(T key){
        int initPos=Hash(key);
        node<T>* pos=data[initPos]->next;
        while (pos && pos->data!=key) pos=pos->next;
        return pos;
    }
    bool Insert(T key){
        node<T>* p=Find(key);
        if(!p){  // 关键词未找到，可以插入
            node<T>* n=new node<T>(key);
            int pos = Hash(key);   // 初始散列表地址
            n->next = data[pos]->next;
            data[pos]->next=n;
            return true;
        }
        return false;
    }
};


int main(){
    Map<int,int>* m=new Map<int,int>(10);
    for (int i=0;i<4;i++) m->Insert(i,i);
    cout << m->Find(1) << endl;
    cout << m->Find(2) << endl;
    m->Delete(1);
    cout << m->Find(1) << endl;
}
```

