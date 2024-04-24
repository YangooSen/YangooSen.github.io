---
title: heap_huffman_uf
katex: true
date: 2024-04-22 21:25:30
excerpt: 最大/小堆,huffman编码，并查集和应用
tag: ds
---

[7-29 修理牧场](https://pintia.cn/problem-sets/15/exam/problems/856) [7-29 修理牧场 (25分)-优先队列+贪心算法+逆向思维](https://blog.csdn.net/weixin_43047175/article/details/106814509?ops_request_misc=&request_id=&biz_id=102&utm_term=7-29%20%E4%BF%AE%E7%90%86%E7%89%A7%E5%9C%BA&utm_medium=distribute.pc_search_result.none-task-blog-2~all~sobaiduweb~default-4-106814509.142%5Ev62%5Epc_search_tree,201%5Ev3%5Eadd_ask,213%5Ev1%5Et3_esquery_v2&spm=1018.2226.3001.4187)，[\[编程题\]搬水果](https://www.nowcoder.com/questionTerminal/e4c775b0f3ee42a4bb72c26d2e1eef8a) **有重复累加时保证小的那些值累加次数多点，大的那些值累加次数小点，哈弗曼编码的思路**
[7-39 魔法优惠券](https://pintia.cn/problem-sets/15/exam/problems/866)，遇到没有完整解决思路的就先写一下示例的解决方法，再慢慢调整，万一对了呢 V^V

```cpp
#include <bits/stdc++.h>
using namespace std;
//注意如果在类里面重载()成为仿函数，记得开class public
bool cmp(const int v1,const int v2){
    return abs(v1)>abs(v2);
}

int main(){
    int n,m,t;
    scanf("%d",&n);
    vector<int> lessZeroTicket;
    vector<int> geZeroTicket;
    for (int i=0;i<n;i++){
        scanf("%d",&t);
        if (t<0) lessZeroTicket.push_back(t);
        else geZeroTicket.push_back(t);
    }
    scanf("%d",&m);
    vector<int> goods(m);
    for (int i=0;i<m;i++) scanf("%d",&goods[i]);
    sort(lessZeroTicket.begin(),lessZeroTicket.end());
    sort(geZeroTicket.begin(),geZeroTicket.end(),greater<int>());
    //把ticket正的和负的分开
    sort(goods.begin(),goods.end(),cmp);
    //对每个免费goods,如果它大于0就找最大的tickes用掉
    //如果是负的就找负的最大的用掉,应该以绝对值排序
    //cout << goods[0];
    long long res=0;
    int GZIndex=0,LZIndex=0;
    for (int i=0;i<m;i++){
        if (goods[i]>0){
            if (GZIndex<geZeroTicket.size()) res+=goods[i]*geZeroTicket[GZIndex++];
            else res+=goods[i];
        }
        else{
            if (LZIndex<lessZeroTicket.size()) res+=goods[i]*lessZeroTicket[LZIndex++];
        }
    }
    cout << res;
}
```
[7-25 朋友圈](https://pintia.cn/problem-sets/15/exam/problems/840)，第一次用图处理内存超限，求最大连通分量可以通过并查集解决，树规模（连通分量）用负数存储即可

```cpp
/*
#include <bits/stdc++.h>
using namespace std;
vector<vector<int>> graph;
vector<bool> collected;
int cnt=0;  //记录每一个连通分量的点数
void print(){
    for (int i=0,len=graph.size();i<len;i++){
        for (int j=0;j<len;j++) cout << graph[i][j] << ' ';
        cout << endl;
    }
}

void dfs(int v){
    collected[v]=true;
    cnt++;
    for (int i=0,len=graph.size();i<len;i++){
        if (graph[v][i] && !collected[i]) dfs(i);
    }
}

int main(){
    //找到最大的连通分量的点数
    int n,m,tmp,v1,v2;
    scanf("%d%d",&n,&m);
    graph.resize(n);
    
    for (int i=0;i<n;i++){
        for (int j=0;j<n;j++) graph[i].push_back(0);
    }
    
    collected.resize(n);
    //for (int i=0;i<n;i++) cout << collected[i];
    for (int i=0;i<m;i++){
        scanf("%d",&tmp);
        vector<int> circle(tmp,0);
        for (int j=0;j<tmp;j++) scanf("%d",&circle[j]);
        //for (int j=0;j<tmp;j++) cout << circle[j];
        //cout << endl;
        //把circle中的人在图中互连
        for (int j=0;j<tmp;j++){
            for (int k=0;k<tmp;k++) graph[circle[j]-1][circle[k]-1]=1;
        }
    }
    int res=-1;
    for (int i=0;i<n;i++){
        if (!collected[i]){
            dfs(i);
            if (cnt>res) res=cnt;
            cnt=0;
        }
    }
    printf("%d",res);
}
*/
//下面的是并查集AC
#include <bits/stdc++.h>
using namespace std;
class UF{
public:
    vector<int> parent; 
    UF(int x){
        parent.resize(x);
        for (int i=0;i<x;i++) parent[i]=-1;
    }
    int Find(int x){
        if (parent[x]>=0) return parent[x]=Find(parent[x]);
        return x;   //返回根
    }
    void Union(int x1,int x2){
        int r1=Find(x1);
        int r2=Find(x2);
        if (r1==r2) return;
        if (parent[r1]<parent[r2]){  //树1规模更大
            parent[r1]+=parent[r2];
            parent[r2]=r1;
        }
        else{
            parent[r2]+=parent[r1];
            parent[r1]=r2;
        }
    }
    int getRes(){
        int res=-2;
        for (int i=0,len=parent.size();i<len;i++){
            if (-parent[i]>res) res=-parent[i];
        }
        return res;
    }
};

int main(){
    //找到最大的连通分量的点数
    int n,m,tmp,v1,v2;
    scanf("%d%d",&n,&m);
    UF u(n);
    for (int i=0;i<m;i++){
        scanf("%d%d",&tmp,&v1);
        for (int j=1;j<tmp;j++){
            scanf("%d",&v2);
            u.Union(v1-1,v2-1);
            //printf("%d %d 连接,res=%d\n",v1,v2,u.getRes());
            v1=v2;
        }
    }
    printf("%d",u.getRes());
}
```
Kruskal 算法也用到了并查集，给定某阶段的Kruskal 算法状态也可用并查集解决 [7-50 畅通工程之局部最小花费问题](https://pintia.cn/problem-sets/15/exam/problems/897)
```cpp
#include <bits/stdc++.h>
using namespace std;
vector<vector<int>> graph;
class UF{
public:
    vector<int> parent;//parent[i]是自己的父节点,根是-规模
    int cnt;    //连通分量的个数
    UF(int n){
        for (int i=0;i<n;i++) parent.push_back(-1);
        cnt=n;
    }
    int Find(int x){
        //返回x对应的根节点
        if (parent[x]>=0) return parent[x]=Find(parent[x]);//这里是>=,而且也要return
        else return x;//return 的是x,因为是根节点下标,不可能是负数
    }
    void Union(int x1,int x2){
        int root1=Find(x1);
        int root2=Find(x2);
        if (root1==root2) return;
        if (parent[root1]<parent[root2]){
            //树1的规模更大
            parent[root1]+=parent[root2];
            parent[root2]=root1;
            cnt--;
        }
        else{
            //树2的规模更大
            parent[root2]+=parent[root1];
            parent[root1]=root2;
            cnt--;
        }
    }
    bool connected(int x1,int x2){
        int root1=Find(x1);
        int root2=Find(x2);
        return root1==root2;
    }
};
class edge{
public:
    int v1,v2,weight;
    edge(int _v1,int _v2,int _w):v1(_v1),v2(_v2),weight(_w){};
    bool operator<(const edge& e) const{
        return e.weight<weight;
    }
};
int main(){
    //给定一些森林(连通分量),要合并成大森林,kruskal算法
    //并查集存储这些连通分量,每次挑出权值最小的边合并两个连通分量
    int n,v1,v2,w,mode;
    scanf("%d",&n);
    UF u(n);
    priority_queue<edge> q;
    int len=n*(n-1)/2;
    for (int i=0;i<len;i++){
        scanf("%d%d%d%d",&v1,&v2,&w,&mode);
        //cout << v1 << ' ' << v2 << ' ' << w << ' ' << mode << endl;
        if (mode==1) u.Union(v1-1,v2-1);
        //已经修好了,直接连接分量,不需要管费用
        else q.push(edge(v1-1,v2-1,w));
    }
    //每次找到权值最小的边,如果边对应的两个连通分量没有连接,就连接,否则丢掉
    //直到图连通
    int res=0;
    while (u.cnt>1){
        edge e=q.top();q.pop();
        if (!u.connected(e.v1,e.v2)){
            u.Union(e.v1,e.v2);
            res+=e.weight;
        }
    }
    printf("%d",res);
}
```
[\[编程题\]Head of a Gang](https://www.nowcoder.com/questionTerminal/a31b1ea6c87647bc86e382acaf21c53b)，添加了信息映射的 unordered_map 即可套用并查集的模板，如果有更多信息可以定义 struct 来映射（见下面的题，**用指针即可**），**注意初始化和添加名字的 insertName 函数，在 Union 之前先插入名字，再用映射合并即可**
```cpp
#include<bits/stdc++.h>
using namespace std;
int n,k,w;
string n1,n2;
class UF{
public:
    int id;
    unordered_map<string,int>  name2int;    //名字映射到下标
    vector<string> int2name;
    vector<int> weight;
    vector<int> parent;
    UF(int maxsize){
        id=0;
        for (int i=0;i<maxsize;i++){
            parent.push_back(-1);
            weight.push_back(0);
            int2name.push_back("$");
        }
    }
    void insertName(const string& s){
        name2int[s]=id;
        int2name[id]=s;
        id++;
    }
    int _find(int x){
        if (parent[x]>=0) return parent[x]=_find(parent[x]);
        return x;
    }
    void Union(const string& s1,const string& s2){
        if (name2int.find(s1)==name2int.end()) insertName(s1);
        if (name2int.find(s2)==name2int.end()) insertName(s2);
        //插入新名字之后parent[id]一定是-1
        _union(name2int[s1],name2int[s2]);
    }
    void _union(int x1,int x2){
        int root1=_find(x1);
        int root2=_find(x2);
        //cout << x1 << ".root=" << root1 << endl;
        //cout << x2 << ".root=" << root2 << endl;
        if (root1==root2) return;
        if (parent[root1]<=parent[root2]){
            //root1的规模更大
            parent[root1]+=parent[root2];
            parent[root2]=root1;
        }
        else{
            parent[root2]+=parent[root1];
            parent[root1]=root2;
        }
    }
    void addWeight(const string& s,int w){
        weight[name2int[s]]+=w;
    }
    void printGang(){
        map<int,vector<int>> gang;//key是集合的根节点,value是帮派成员的id
        for (int i=0;i<id;i++){
            int root=_find(i);
            //cout << id << ".root=" << root << endl;
            gang[root].push_back(i);
        }
        for (auto x:gang){
            cout << int2name[x.first] << ' ';
            for (auto y:x.second){
                cout << int2name[y] << ' ' << weight[y] << endl;
            }
            cout << endl;
        }
    }
    void process(){
        //printGang();
        map<int,vector<int>> gang;//key是集合的根节点,value是帮派成员的id
        for (int i=0;i<id;i++){
            int root=_find(i);
            //cout << id << ".root=" << root << endl;
            gang[root].push_back(i);
        }
        //统计每个gang的信息
        map<string,int> res;    //key是头领名字
        for (auto x:gang){
            if (x.second.size()>2){
                int sum=0;
                int maxWeight=INT32_MIN,headerID;
                //人数大于2并且权重之和大于k
                for (int memberID:x.second){
                    if (weight[memberID]>maxWeight){
                        maxWeight=weight[memberID];
                        headerID=memberID;
                    }
                    sum+=weight[memberID];
                }
                //gang内每个人通话记录只算一次
                if (sum>k*2) res[int2name[headerID]]=x.second.size();
            }
        }
        
        cout << res.size() << endl;
        for (auto x:res){
            cout << x.first << ' ' << x.second << endl;
        }
        
    }
};
int main(){
    while (cin >> n >> k){
        UF u(2000);
        for (int i=0;i<n;i++){
            cin >> n1 >> n2 >> w;
            u.Union(n1,n2);
            u.addWeight(n1,w);
            u.addWeight(n2,w);
        }
        u.process();
    }
}
```
[并查集+最小生成树 Kruskal ](https://www.nowcoder.com/questionTerminal/41b14b4cd0e5448fb071743e504063cf)用到了并查集的结构体映射，因为必须是可哈希对象或者提供哈希函数，**key 用指针即可**，还有重载 cmp 仿函数作为优先队列的参数，注意插入结构体的操作
```cpp
#include<bits/stdc++.h>
//#include<iomanip>
using namespace std;
//Kruskal算法解决最小生成树
struct point{
    double x,y;
    point(){};    //无参构造函数这里要写上,不然edge那里要报错
    point(double _x,double _y):x(_x),y(_y){};
};
struct edge{
    point *x;
    point *y;
    double weight;
    edge(point* p1,point* p2){
        x=p1;
        y=p2;
        weight=sqrt(((y->x-x->x)*(y->x-x->x))+((y->y-x->y)*(y->y-x->y)));
    }
};
struct cmp{
    bool operator()(const edge& e1,const edge& e2){
        return e1.weight>e2.weight;
    }
};
class UF{
public:
    vector<point*> int2item;
    unordered_map<point*,int> item2int;
    vector<int> parent;
    int id;
    int cnt;    //连通分量
    UF(int maxsize){
        id=0;
        int2item.resize(maxsize);
        for (int i=0;i<maxsize;i++){
            parent.push_back(-1);
        }
    }
    
    int find(int x){
        if (parent[x]>=0) return parent[x]=find(parent[x]);
        return x;
    }
    void insertPoint(double x,double y){
        point* p=new point(x,y);
        item2int[p]=id;
        int2item[id]=p;
        id++;
    }
    bool connected(point* p1,point* p2){
        int root1=find(item2int[p1]);
        int root2=find(item2int[p2]);
        return root1==root2;
    }
    void Union(int x1,int x2){
        int root1=find(x1);
        int root2=find(x2);
        if (root1==root2) return;
        if (parent[root1]<=parent[root2]){
            //root1的规模更大
            parent[root1]+=parent[root2];
            parent[root2]=root1;
        }
        else{
            parent[root2]+=parent[root1];
            parent[root1]=root2;
        }
        cnt--;    //合并之后连通分量--
    }
    void kruscal(){
        //产生最小生成树
        priority_queue<edge,vector<edge>,cmp> q;
        //计算所有边
        int len=id;
        //cout << len << endl;
        for (int i=0;i<len;i++){
            for (int j=i;j<len;j++){
                if (i==j) continue;
                edge e(int2item[i],int2item[j]);
                q.push(e);
            }
        }
        //cout << q.size() << endl;
        /*
        while (!q.empty()){
            cout << q.top().weight << endl;
            q.pop();
        }
        */
        std::cout.setf(ios::fixed);
        std::cout.precision(2);
        double sum=0.0;
        cnt=id;    //连通分量初始化
        while (cnt>1){
            edge e=q.top();q.pop();
            //cout << e.weight << endl;
            
            if (!connected(e.x, e.y)){
                Union(item2int[e.x],item2int[e.y]);
                sum+=e.weight;
            }
            
        }
        cout << sum;
    }
};
int main(){
    int n;
    double x,y;
    UF u(105);
    cin >> n;
    for (int i=0;i<n;i++){
        cin >> x >> y;
        u.insertPoint(x,y);
    }
    u.kruscal();
}
```
```cpp
#include <iostream>
#include <unordered_map>
#define MAXDATA 1000
#define MINDATA -1000
#define MAXSIZE 100
#include <queue>
#include <utility>  //pair类
using namespace std;

string path="";   //记录从根到叶子节点的遍历路径

template<typename T>
class maxHeap{
    public:
    //取出元素按照关键字(优先权)大小,而不是元素入队顺序
    //priorityQueue 或maxHeap,必须是完全二叉树,根节点比孩子节点大或小
    //数组存储
    T *Array;        //△元素从下标为1的地方开始,下标0不存元素△
    int size;       //堆最后一个元素的下标
    int capacity;   //最大的元素下标
    maxHeap(int maxsize){
        size=0;   //这里就是从0开始,不需要从1开始,0表示是空堆
        capacity=maxsize+1; //△注意这里是maxsize+1△
        Array=(T *)malloc((maxsize+1) * sizeof(T));  //注意初始化方法,malloc
        Array[0]=(T)MAXDATA;
    };
    //MAXDATA的所有可能元素中的最大值,作为哨兵,方便方法实现
    maxHeap(T data[],int N){
        //堆排序
        //将已经存在的N个元素建成一个最大堆,如果直接调用Insert,O(NlogN)
        //下面是线性复杂度的算法,思路是现将它们顺序存放,之后再调整成最大堆
        Array=(T *)malloc((N+1) * sizeof(T));  //注意初始化方法,malloc
        Array[0]=(T)MAXDATA;
        for (int i=0;i<N;i++) Array[i+1]=data[i];
        //for (int i=1;i<N;i++) cout << Array[i] << ',';cout << endl;
        size=capacity=N;    //最大数组下标是N,总共N+1个元素
        int i=size/2;    //倒数第一个不是最大堆的堆的根节点
        for(;i>0;i--){
		// 以每个有孩子结点的结点作为根结点，对其子树进行堆排序
            adjust(i);
        }

    }
    void adjust(int i){
        //将i为根节点的堆调成最大堆,类似Delete操作
        //和Delete的差别是adjust是给根节点找位置,Delete是给最后一个元素找位置
        T tmp=Array[i]; //拿到根节点的值,根的位置空出,接下来把堆中最大的元素放到根上
        int parent,child; //parent始终指向空位
        for (parent=i;parent*2<=size;parent=child){ //parent*2<=size判断是否有左儿子
            child=parent*2;
            if (child!=size && Array[child]<Array[child+1]) child++; //有右儿子并且右儿子的值更大
            if (tmp>=Array[child]) break;  //child指向parent最大的儿子,tmp比parent左右儿子都大,tmp可以放到这里
            else Array[parent]=Array[child]; //否则把大儿子放上去,让出位置,继续下一轮比较(parent=child),parent从1一层层向堆底走
        }
        Array[parent]=tmp;
    }
    //忘记+
    void Insert(T x){
    	//依次向下过滤
        int child;
        if (size==capacity){
            cout << "最大堆已满" << endl;
            return;
        }
        child=++size;
        //child开始指向空位,第一次遍历是将空位的父元素挪到那个空位上
        for(;Array[child/2]<x;child/=2) Array[child]=Array[child/2]; //把父节点挪下来,空出位置,哨兵可以保证永远不会越界
        Array[child]=x;
    }
    T Delete(){
        //拿出最大元素,元素依次向上过滤,可以保证完全二叉树
        int parent,child;
        if (size==0){
            cout << "最大堆已空" << endl;
            return;
        }
        T maxItem=Array[1];   //要删除的第一个元素
        T tmp=Array[size--]; //拿到最后一个元素
        for (parent=1;parent*2<=size;parent=child){ //parent*2<=size判断是否有左儿子
            child=parent*2;
            if (child!=size && Array[child]<Array[child+1]) child++; //有右儿子并且右儿子的值更大
            if (tmp>=Array[child]) break;  //child指向parent最大的儿子,tmp比parent左右儿子都大,tmp可以放到这里
            else Array[parent]=Array[child]; //否则把大儿子放上去,让出位置,继续下一轮比较(parent=child),parent从1一层层向堆底走
        }
        Array[parent]=tmp;
        return maxItem;
    }
    void print(){
        for (int i=1;i<=size;i++){
            cout << Array[i] << ' ';
        }
    }
};

//下面是利用堆和哈夫曼树实现哈夫曼最优编码
//堆中的元素是哈夫曼树
//每个节点是一个pair,first是待编码字符,second是字符出现频率
//合并的时候非叶子节点不对应字符,但占据了节点,这里用字符N代表
class huffmanTree{
    public:
    huffmanTree* left;
    huffmanTree* right;
    pair<string,int> data;
    huffmanTree(int freq):left(nullptr),right(nullptr),data("N",freq){}; //默认不编码字符是N,权重(出现频率)需要合并才能得到
    huffmanTree(string c,int freq):left(nullptr),right(nullptr),data(c,freq){}; //编码的字符节点
    //层序遍历
    void levelTraversal(){
        queue<huffmanTree*> q;
        q.push(this);
        while (!q.empty()){
            huffmanTree* cur=q.front();q.pop();
            cout << cur->data.first << ":" << cur->data.second << endl;
            if (cur->left) q.push(cur->left);
            if (cur->right) q.push(cur->right);
        }
    }
};
class minHeap{
    public:
    int size;
    int maxSize;
    huffmanTree** Array; //哈夫曼树指针的数组
    minHeap(int max_size){
        size=0;
        maxSize=max_size;
        Array=(huffmanTree**)malloc(sizeof(struct huffmanTree)*(max_size+1));
        Array[0]=new huffmanTree(MINDATA); //哨兵,最小值
    }
    void Insert(huffmanTree* tree){
        //依据树node.second排序的树,根是最小值
        if (size==maxSize){
            cout << "最小堆已满" << endl;
            return;
        }
        //先创造一个空位
        int child=++size;
        //从下往上比较
        for (;Array[child/2]->data.second>tree->data.second;child/=2) Array[child]=Array[child/2]; //父节点挪到空位
        Array[child]=tree;
    }
    huffmanTree* Delete(){
        if (size==0){
            cout << "最小堆已空" << endl;
            return nullptr;
        }
        huffmanTree* minItem=Array[1];      //拿掉最小的值
        //从上往下比较,把下面更小的儿子节点挪上去
        int parent,child;
        huffmanTree* tmp=Array[size--];     //拿到最后一个节点,现在就是在给tmp找位置放
        for (parent=1;parent*2<=size;parent=child){ //有左节点
            child=parent*2;
            if (child+1<=size && Array[child+1]->data.second<Array[child]->data.second) child++;
            if (tmp->data.second<Array[child]->data.second) break; //tmp可以放到现在parent的节点了,因为它比儿子都小
            else Array[parent]=Array[child];
        }
        Array[parent]=tmp;
        return minItem;
    }
    void print(){
        for (int i=1;i<=size;i++) cout << Array[i]->data.first << ':' << Array[i]->data.second << endl;
    }
};

void backTracking(huffmanTree* node){
    //根据哈夫曼树编码字符
    if (!node->left && !node->right){       //node->data.first如果不是N也可以作为编码依据
        //叶子结点,编码字符串,输出此时的path,即从根遍历到该叶子结点的路径
        cout << node->data.first << ":" << path << endl;
        return;
    }
    //继续遍历下一层
    path+="0";backTracking(node->left);
    path.pop_back(); //回溯
    path+="1";backTracking(node->right);
    path.pop_back(); //这里也得回溯
}

void codeString(unordered_map<string,int>& char2freq){
    //把数据变成树节点,插入最小堆
    int len=char2freq.size();
    huffmanTree* data[len];
    unordered_map<string,int>::iterator it=char2freq.begin();
    for (int i=0;i<len;i++){
        huffmanTree* cur=new huffmanTree(it->first,it->second);
        it++;
        data[i]=cur;
    }
    //插入最小堆
    minHeap* h=new minHeap(MAXSIZE);
    for (int i=0;i<len;i++){
        h->Insert(data[i]);
    }
    //h->print();
    //找出最小的树合并后再插入堆,总共有len个元素,需要合并len-1次
    for (int i=1;i<len;i++){
        huffmanTree* l=h->Delete();
        huffmanTree* r=h->Delete();
        huffmanTree* mergeTree=new huffmanTree(l->data.second+r->data.second); //合并频率(权重)相加,不编码字符,以N标记
        mergeTree->left=l;mergeTree->right=r;
        h->Insert(mergeTree);
    }
    huffmanTree* res=h->Delete();
    //res->levelTraversal();     //和视频中结果一致
    //下面用回溯法遍历整棵树,输出01编码字符
    backTracking(res);          //和视频中结果一致
}


int main(){
    //int data[8]={55, 66, 44, 33, 11, 22, 88, 99};
    //maxHeap<int>* h=new maxHeap<int>(MAXSIZE);
    //for (int i=0;i<8;i++){
    //    h->Insert(data[i]); //O(NlogN)
    //}

    //maxHeap<int>* h=new maxHeap<int>(data,8); //用调整方法建的堆可能与一个个插入构造的堆不同,建立速度更快O(N)
    //h->print();
    unordered_map<string,int> char2freq={{"a",10},{"e",15},{"i",12},{"s",3},{"t",4},{"sp",13},{"nl",1}}; //字符出现频率
    /* 利用哈夫曼树编码结果应该如下:
        a:111   e:10    i:00    s:11011
        t:1100  sp:01   nl:11010
    */
    codeString(char2freq);
    return 0;
}

template<typename T>
class UF{
public:
    unordered_map<T,int> item2Int; //实现从任何对象到整数的映射,空间换时间
    int cnt; //连通分量个数
    int* parent;    //i对应的那个对象的父节点是parent[i],如果自己是父节点parent[i]表示的是树高的相反数
    int maxSize;   //parent数组的最大索引是maxSize-1
    UF(int max_size,T dataset[],int len){
        maxSize=max_size;
        //初始化映射map
        for (int i=0;i<len;i++) item2Int[dataset[i]]=i;
        parent=new int[maxSize];
        for (int i=0;i<maxSize;i++) parent[i]=-1; //初始全不连通
        cnt=len;
    }
    int Find(T x){
        if (!item2Int.count(x)){
            cout << "查询元素不存在";
            return -1;
        }
        int i=item2Int[x];
        if (parent[i]>=0){ //不是根
            return parent[i]=_Find(parent[i]); //路径压缩,i的父节点直接指向i的祖宗(根),省去从父亲到爷爷等的路径
        }
        return i; //
    }
    int _Find(int i){
        //实现映射的查找
        if (parent[i]!>=0){	//这里忘了☆☆☆☆☆☆☆☆
            return parent[i]=_Find(parent[i]);
        }
        return i;
    }
    //按秩归并
    void Union(T x1,T x2){
        int root1=Find(x1);
        int root2=Find(x2);
        if (parent[root1]<parent[root2]){  //root1更高
            parent[root2]=root1;   //小树的父节点是大树
        }
        else if (parent[root1]>parent[root2]){  //root2更高
            parent[root1]=root2;   				//小树的父节点是大树
        }
        else{
        	//树高相同
        	parent[root1]=root2;  //把root1挂到root2下面
        	parent[root2]--;  //root2树高+1,因为树高是相反数所以是--
        }
        cnt--;
    }
    bool connected(T x1,T x2){
        int root1=Find(x1);
        int root2=Find(x2);
        return root1==root2;
    }
    int getCnt(){
        return cnt;
    }
};

int main(){
    int len=5;
    int max_size=100;
    int dataset[len];
    for (int i=0;i<len;i++){
        dataset[i]=i;  //编号是0-4,与视频中不同
    }
    UF<int>* ufset=new UF<int>(max_size,dataset,len);
    char c='z';
    int m1,m2;
    while (c!='S'){
        cin >> c;
        if (c!='S') cin >> m1 >> m2; //视频中编号是1-5,为了统一下面全部-1
        else break;
        if (c=='C')
            if (ufset->connected(m1-1,m2-1)) cout << "yes" << endl;
            else cout << "no" << endl;
        if (c=='I') ufset->Union(m1-1,m2-1);
    }
    cout << ufset->cnt;
}

```
