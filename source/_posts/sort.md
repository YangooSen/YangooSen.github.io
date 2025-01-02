---
title: sort
katex: true
date: 2024-12-30 21:19:27
excerpt: 排序算法
tag: ds
---


```cpp
#include <iostream>
#define CUTOFF 3
using namespace std;

//从小到大排序,定义了">=<",只讨论内部排序,考察排序算法的稳定性
//△△△需要swap的地方不要直接拷贝△△△

template<typename T>
void Swap(T &a,T &b){
    T tmp=a;
    a=b;
    b=tmp;
}

template<typename T>
void bubleSort(T a[],int n){
	//忘了TvT
    //最好O(N),最坏(O^2),当元素放在单向链表仍然很合适
    for (int End=n-1;End>=0;End--){ //这一轮结束要保证a[End]有正确的值
        bool succeed=true;          //如果没有任何交换则已经有序
        for (int i=0;i<End;i++){  //下面已经有a[i+1],不需要i<=End,
            //从上向下看,i在上面,i+1在下面
            if (a[i+1]<a[i]){
            //上面的泡泡更大,应该沉下去,一直遍历可以沉到底
            //这里没有等号,因此是稳定排序
                Swap(a[i+1],a[i]);
                succeed=false;
            }
        }
        if (succeed) break;
    }
}

template<typename T>
void insertionSort(T a[],int n){
    //最好O(N),最坏完全逆序O(N^2)
    for (int curIndex=1;curIndex<n;curIndex++){     //从第二章牌开始拿
        T cur=a[curIndex]; //现在拿到的牌,curIndex留出了空位
        int should=curIndex;  //cur应该放的位置,始终保持它是空位
        for (;should-1>=0 && a[should-1]>cur;should--) a[should]=a[should-1]; //一开始should是空位,因此是should-1放到should的位置
        a[should]=cur;      //注意上面是>=1,后面有should-1
    }
}

//对于从小到大来说,i<j且a[i]>a[j]是逆序对,冒泡排序和插入排序本质是消除逆序对
//N个不同元素的序列平均逆序对有N(N-1)/4个,仅仅交换相邻元素的算法平均复杂度是Ω(N^2)(下界,最好是N^2)
//插入排序O(N+I),如果序列基本有序插入排序较快

template<typename T>
void shellSort(T a[],int n){
    //定义增量序列,对每个增量序列进行排序
    //执行D_{k-1}间隔排序时能保证D_{k}间隔仍然有序,继续排序永远不会变坏
    //增量序列互质更好,比如Hibbard和Sedgewick序列,实现时只需要把d换成相应序列增量即可
    for (int d=n/2;d>0;d/=2){   //最终必须进行一次1间隔排序,此时序列已经基本有序,插入排序很快
        //下面就是插入排序,只是把1换成了d,即每次移动间隔d,而不是1
        //☆☆☆☆curIndex和curIndex-d比较,从d开始,每次+1☆☆☆
        for (int curIndex=d;curIndex<n;curIndex++){
            T cur=a[curIndex];      //现在拿到的牌,curIndex留出了空位
            int should=curIndex;  //cur应该放的位置,始终保持它是空位
            for (;should-d>=0 && a[should-d]>cur;should-=d) a[should]=a[should-d]; //一开始should是空位,因此是should-1放到should的位置
            a[should]=cur;      //注意上面是>d,后面有should-d
        }
    }
}

template<typename T>
void selectionSort(T a[],int n){
    //找到最小元素后放到有序位置的最后部分,noSortedIndex之前的元素都已经有序
    for (int noSortedIndex=0;noSortedIndex<n;noSortedIndex++){ //a[noSortedIndex]还是无序
        int minIndex=-1;
        T minElement=T(INT32_MAX);
        for (int i=noSortedIndex;i<n;i++){
            if (a[i]<minElement){
                minElement=a[i];
                minIndex=i;
            }
        }
        //△△△需要swap的地方不要直接拷贝△△△
        Swap(a[minIndex],a[noSortedIndex]); //把最小元素放到有序部分的最后为准
    }
}

template<typename T>
void adjust(T a[],int root,int n){
	//忘了TvT,记住☆☆☆函数签名☆☆☆
    //将以root为根节点的堆调整成最大堆
    T tmp=a[root];
    int parent,child;   //parent始终指向tmp的位置
    for (parent=root;parent*2<n;parent=child){
        child=parent*2;
        if (child+1<n && a[child+1]>a[child]) child++; //child始终指向大的那个儿子
        if (tmp>=a[child]) break;   //△这里是>=△
        else a[parent]=a[child];
    }
    a[parent]=tmp;
}

template<typename T>
void heapSort(T a[],int n){
	//把a[]看成一个堆即可,不需要额外实现堆数据结构
    //比O(NlogN)好一些,实际效果不如Sedgewick序列的希尔排序
    //先把数组调整成最大堆,然后交换根(最大值)和最后一个元素,这样最大值就放到了正确位置
    for (int root=n/2;root>=0;root--) adjust(a,root,n);     //建成最小堆
    //noSortedIndex之后的元素都已经有序
    for (int noSortedIndex=n-1;noSortedIndex>0;noSortedIndex--){ //把根节点也就是最大的值放到正确的位置,这轮循环保证a[noSortedIndex]有正确的值
        Swap(a[0],a[noSortedIndex]);
        //只是交换了根节点,左右节点还是最大堆,只需要调整根节点0就可以保证整个堆是最大堆
        adjust(a,0,noSortedIndex);	//注意这里不是n,交换之后相当于把noSortedIndex移除了
    }

}
template<typename T>
void Merge(T a[],T tmp[],int leftStart,int rightStart,int rightEnd){
    //把[a[leftStart],a[leftEnd]]和[[a[rightStart],a[rightEnd]]]合并成有序序列
    int leftEnd=rightStart-1;
    int left=leftStart;  //保存起点,之后需要导入a
    int tmpStart=leftStart;
    for (;leftStart<=leftEnd && rightStart<=rightEnd;){
        if (a[leftStart]<=a[rightStart]) tmp[tmpStart++]=a[leftStart++];
        else tmp[tmpStart++]=a[rightStart++];
    }
    while (leftStart<=leftEnd) tmp[tmpStart++]=a[leftStart++];
    while (rightStart<=rightEnd) tmp[tmpStart++]=a[rightStart++];
    for (;left<=rightEnd;left++) a[left]=tmp[left];
}

template<typename T>
void Merge1(T a[],T tmp[],int leftStart,int rightStart,int rightEnd){
    //把[a[leftStart],a[leftEnd]]和[[a[rightStart],a[rightEnd]]]合并成有序序列
    int leftEnd=rightStart-1;
    int tmpStart=leftStart;
    for (;leftStart<=leftEnd && rightStart<=rightEnd;){
        if (a[leftStart]<=a[rightStart]) tmp[tmpStart++]=a[leftStart++];
        else tmp[tmpStart++]=a[rightStart++];
    }
    while (leftStart<=leftEnd) tmp[tmpStart++]=a[leftStart++];
    while (rightStart<=rightEnd) tmp[tmpStart++]=a[rightStart++];
    //最后一步不重新倒回a
}

template<typename T>
void _mergeSort(T a[],T tmp[],int left,int right){
    //把[a[left],a[right]]调整成有序
    if (left<right){    //☆注意这里是if,不是while,当left==right的时候[a[left],a[right]]不是一个合格的区间,因此没有=☆
        int center=left+((right-left)>>1);
        _mergeSort(a,tmp,left,center);
        _mergeSort(a,tmp,center+1,right);
        Merge(a,tmp,left,center+1,right);
    }
}

template<typename T>
void mergeSort(T a[],int n){
    //稳定的排序算法,最坏都是O(NlogN),外排序的时候很有用
    int *tmp=new int[n];
    _mergeSort(a,tmp,0,n-1);
    delete tmp;  //△不要忘了释放空间△
}

template<typename T>
void mergePass(T a[],T tmp[],int n,int length){
    //一趟归并,总共要做log(n)趟,归并之后tmp有序,a没有动
    int start; //记录归并做到哪里了
    //每个子序列的长度都是length,两两合并成有序子序列
    //start=0,length=2的时候就是合并{a[0],a[1]}和{a[2],a[3]}
    //每次合并[start,start+length-1]和[start+length,start+length*2-1]
    for (start=0;start<n-2*length;start+=2*length) Merge1(a,tmp,start,start+length,start+2*length-1);
    if (start+length<n) Merge(a,tmp,start,start+length,n-1); //归并剩下两个子列
    else for (int i=start;i<n;i++) tmp[i]=a[i];  //只剩一个子列直接导入
}

template<typename T>
void noRecMergeSort(T a[],int n){
    //非递归归并排序
    int* tmp=new int[n];
    //每次长度加倍
    int length=1;
    while (length<n){
        mergePass(a,tmp,n,length); //有序的在tmp内
        length*=2;
        mergePass(tmp,a,n,length); //有序的在a内
        length*=2;
    }
    delete tmp;
}

template<typename T>
T Median3(T a[],int left,int right){
    //返回主元,它是头中尾三个数的中间的那个数
    //最终调整成a[left]=min_element,a[right]=max_element,a[right-1]=mid_element
    int mid=left+((right-left)>>1);
    if (a[mid]<a[left]) Swap(a[mid],a[left]);       //这步保证left<mid
    if (a[mid]>a[right]) Swap(a[mid],a[right]);     //这步保证mid<right
    Swap(a[mid],a[right-1]);
    return a[right-1];
}

template<typename T>
void _quickSort(T a[],int left,int right){
    //将[a[left],a[right]]调整好顺序
    if (CUTOFF<right-left){
        //规模大的时候才递归快速排序
        //选主元
        T pivot=Median3(a,left,right);
        //把pivot放到中间
        int i=left,j=right-1;
        for (;;){
            //找到第一个不符合的之后转到右边,两个都不符合就交换
            while (a[++i]<pivot){}
            while (a[--j]>pivot){}
            if (i<j) Swap(a[i],a[j]);
            else break;
        }
        Swap(a[i],a[right-1]);
        _quickSort(a,left,i-1);
        _quickSort(a,i+1,right);
    }
    /*
    //bubleSort(a,n);
    //insertionSort(a,n);
    //shellSort(a,n);
    //selectionSort(a,n);
    //heapSort(a,n);
    //mergeSort(a,n);
    //noRecMergeSort(a,n);
    */
    else noRecMergeSort(a+left,right-left+1);  //递归的mergeSort会出问题?
}

template<typename T>
void quickSort(T a[],int n){
    //选主元,递归治理左右两边
    //可以选头,中,尾中位数
    //大规模数据用递归的代价很大,可以在低于阈值的时候用简单排序即可
    _quickSort(a,0,n-1);
}

template<typename T>
void tableSort(T a[],int n){
    //O(mN),m是移动元素花费的时间
    //看视频好理解一些
    //只做指针的移动,不移动真实元素
    //N个数字的排列由若干个独立的环
    int indexTable[n];
    for (int i=0;i<n;i++) indexTable[i]=i; //初始化
    //下面进行插入排序,只修改指针indexTable
    for (int curIndex=1;curIndex<n;curIndex++){     //curIndex是table的index
        int cur=a[indexTable[curIndex]].grade;    //都套一层indexTable
        int tmp=indexTable[curIndex];      //tmp是table的值
        int should=curIndex;  //tmp应该放的位置,始终保持它是空位
        for (;should-1>=0 && a[indexTable[should-1]].grade>cur;should--) indexTable[should]=indexTable[should-1];
        indexTable[should]=tmp;          //注意上面是>=1,后面有should-1
    }
    for (int i=0;i<n;i++) cout << a[indexTable[i]].grade << ' ';cout << endl; //如果只要求输出就可以不用物理排序,直接输出
    //下面进行物理排序,物理上真正移动元素
    for (int aIndex=0;aIndex<n;aIndex++){
        if (aIndex==indexTable[aIndex]) continue;
        //进入了一个环
        T tmp=a[aIndex]; //存在临时位置
        //下面物理移动元素
        int emptyAindex=aIndex;
        while (emptyAindex!=indexTable[emptyAindex]){ //下面按照视频中第一轮的比较解释
            a[emptyAindex]=a[indexTable[emptyAindex]];  //emptyAindex=0,indexTable[emptyAindex]=3,这一步把a[3]换到a[0]
            int tmpIndex=indexTable[emptyAindex];   //tmpIndex=3,保存空位
            indexTable[emptyAindex]=emptyAindex;    //修改table[i]=i,这里就是令table[0]=0
            emptyAindex=tmpIndex;       //找到下一个空位,emptyIndex=3
        }
    }
    for (int i=0;i<n;i++) cout << a[i].grade << ' '; //此时已经不需要indexTable的映射了,已经物理交换了元素
}

template<typename T>
class List{
public:
    T data;
    List* next;
    List(T d):data(d),next(nullptr){};
    List():data(),next(nullptr){};
    void tailInsert(T x){
        //尾插法
        //cout << x.grade << ' ';
        List<T>* cur=this;
        //cout << cur->data.grade;
        while (cur->next) cur=cur->next;
        cur->next=new List(x);
    }
};

template<typename T>
void bucketSort(T a[],int n){
    //基于排序的算法总有情况会慢到O(NlogN)
    //O(M+N),M是MAXSIZE,N是这里的n
    int MAXSiZE=10;
    List<T>* bucket[MAXSiZE]; //MAXSIZE是T数据结构中key的最大值
    for (int i=0;i<MAXSiZE;i++) bucket[i]=new List<T>(); //初始化
    for (int i=0;i<n;i++){
        int grade=a[i].grade;
        bucket[grade]->tailInsert(a[i]);
    }
    for (int i=0;i<MAXSiZE;i++){
        if (bucket[i]->next){ //桶不空
            List<T>* cur=bucket[i]->next;
            while (cur){
                cout << cur->data.grade << ' ';
                cur=cur->next;
            }
        }
    }
}

template<typename T>
void radixSort(T a[],int n){
    //O(P(N+B)),可以用于多关键字排序
    //基数排序,以10为基数,放10个桶,下面用次位优先策略
    int radix=10;
    List<T>* buckets[2][radix];     //建立两个桶交替用
    int useBucketTag=0;
    for (int i=0;i<radix;i++) buckets[useBucketTag][i]=new List<T>();
    //先把数放到桶里,必须用△尾插法△
    for (int i=0;i<n;i++){
        int key=a[i].grade%10;  //拿到个位数
        buckets[useBucketTag][key]->tailInsert(a[i]);
    }
    /*
    //第一趟排序后的输出
    for (int i=0;i<radix;i++){
        if (buckets[useBucketTag][i]->next){
            List<T>* cur=buckets[useBucketTag][i]->next;
            while (cur){
                cout << cur->data.grade << ' ';
                cur=cur->next;
            }
        }
    }
    */
    //下面进行log_{radix}M趟,每次扫描前一趟的筒放到对应的桶
    int pass=2;
    int PASS=3;
    useBucketTag=1;

    for (;pass<=PASS;pass++){
        //更新筒,相当于建立了新桶
        for (int i=0;i<radix;i++) buckets[useBucketTag][i]=new List<T>();
        int lastUseBucketTag=useBucketTag==1?0:1; //这里的lastUseBucketTag是上次放的桶
        //要看倒数第pass个数来排序,把排好的数放到useBucketTag那个桶里
        for (int i=0;i<radix;i++){
            if (buckets[lastUseBucketTag][i]->next){ //桶不空就遍历桶
                List<T>* cur=buckets[lastUseBucketTag][i]->next;
                while (cur){
                    int grade=cur->data.grade;
                    //找到grade的倒数第pass个数
                    int cnt=pass-1;
                    while (cnt--) grade/=10; //当pass=2时除1次10,当pass=3时除2次
                    int key=grade%10;
                    //cout << key << endl;
                    //下面头插法查到桶内,实际可以用类方法实现,这里就复制了
                    buckets[useBucketTag][key]->tailInsert(cur->data);
                    cur=cur->next;
                }
            }
        }
        //更改下一次使用的桶
        useBucketTag=lastUseBucketTag;
    }
    //最后把桶内的元素依次输出
    useBucketTag=useBucketTag==1?0:1;
    for (int i=0;i<radix;i++){
        if (buckets[useBucketTag][i]->next){
            List<T>* cur=buckets[useBucketTag][i]->next;
            while (cur){
                cout << cur->data.grade << ' ';
                cur=cur->next;
            }
        }
    }
}


struct student{
    int grade;
    double others=0.0;
    student():grade(-1),others(0.0){};
    student(int g):grade(g),others(0.0){};
};

int main(){
    int n=6;
    int a[]={4,2,5,1,3,6};
    //bubleSort(a,n);
    //insertionSort(a,n);
    //shellSort(a,n);
    //selectionSort(a,n);
    //heapSort(a,n);
    //mergeSort(a,n);
    //noRecMergeSort(a,n);
    //quickSort(a,n);
    //for (int i=0;i<6;i++) cout << a[i] << ' ';


    //student s[n];
    //for (int i=0;i<n;i++) s[i].grade=i;
    //tableSort(s,n);
    //bucketSort(s,n);


    student t[10]={student(64),student(8),student(216),student(512),student(27),student(729),student(0),student(1),student(343),student(125)};
    radixSort(t,10);
    return 0;
}
```
