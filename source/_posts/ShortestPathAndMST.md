---
title: ShortestPathAndMST
katex: true
date: 2024-12-30 21:17:43
excerpt: 最短路径（Dijkstra，Floyd，SPFA）及最小生成树（Prim，Kruskal）
tag: ds
---

```cpp
//以邻接表实现的图和优先队列的Dijkstra 和 关键路径
#include<iostream>
#include<cstdio>
#include<vector>
#include<cstring>
#include<queue>
#include<climits>

using namespace std;

const int MAXN=200;
const int INF=INT_MAX;

struct Edge{
	int to;
	int length;
	Edge(int t,int l):to(t),length(l) {}	//构造函数
};

struct Point{
	int number;
	int distance;
	Point(int n,int d):number(n),distance(d){}
	bool operator<(const Point& p) const{
		return distance>p.distance;
	}
};

vector<Edge> graph[MAXN];	//使用向量数组表示邻接表，用向量数组的下标表示from

int dis[MAXN];		//表示顶点到所有点的最短路径长度


void Dijkstra(int start,int n){		//起点和顶点个数

	fill(dis,dis+n,INF);
	dis[start]=0;
	priority_queue<Point> myPriorityQueue;
	myPriorityQueue.push(Point(start,dis[start]));
	while(!myPriorityQueue.empty()){
		int u=myPriorityQueue.top().number;
		myPriorityQueue.pop();
		for(int j=0;j<graph[u].size();j++){
			int v=graph[u][j].to;
			int d=graph[u][j].length;
			if(dis[u]+d<dis[v]){
				dis[v]=dis[u]+d;
				myPriorityQueue.push(Point(v,dis[v]));
			}
		}
	}
	return;
}

int main(){
	int n,m;
	while(scanf("%d%d",&n,&m)!=EOF){
		memset(graph,0,sizeof(graph));
		while(m--){
			int from,to,length;
			scanf("%d%d%d",&from,&to,&length);
			graph[from].push_back(Edge(to,length));
			graph[to].push_back(Edge(from,length));
		}
		int start;
		int terminal;
		scanf("%d%d",&start,&terminal);
		Dijkstra(start,n);
		if(dis[terminal]==INF){
			dis[terminal]=-1;
		}
		printf("%d\n",dis[terminal]);
	}
	return 0;
}
#include<iostream>
#include<cstdio>
#include<cstring>
#include<queue>
#include<vector>
#include<climits>

using namespace std;

const int MAXN=1001;
const int INF=INT_MAX;

struct Edge{
    int to;
    int length;
    Edge(int t,int l):to(t),length(l){}
};

vector<Edge> graph[MAXN];//邻接表
int earliest[MAXN];//最早开始时间
int latest[MAXN];//最晚开始时间
int inDegree[MAXN];

void CriticalPath(int n){
    vector<int> topology;//拓扑序列
    queue<int> node;
    for(int i=0;i<n;i++){
        if(inDegree[i]==0){
            node.push(i);
            earliest[i]=1;//初始化为1,题目中的要求：1ns
        }
    }
    while(!node.empty()){
        int u=node.front();
        topology.push_back(u);
        node.pop();
        for(int i=0;i<graph[u].size();i++){
            int v=graph[u][i].to;
            int l=graph[u][i].length;
            earliest[v]=max(earliest[v],earliest[u]+l);//正向找最大，形成earliest
            inDegree[v]--;
            if(inDegree[v]==0) node.push(v);
        }
    }
    for(int i=topology.size()-1;i>=0;i--){
        int u=topology[i];
        if(graph[u].size()==0) latest[u]=earliest[u];//汇点的最晚开始时间初始化
        else latest[u]=INF;//非汇点的最晚开始时间的初始化
        for(int j=0;j<graph[u].size();j++){
            int v=graph[u][j].to;
            int l=graph[u][j].length;
            latest[u]=min(latest[u],latest[v]-l);//逆向找最小，形成latest
        }
    }
}

int main(){
    int n,m;
    while(cin>>n>>m){
        memset(graph,0,sizeof(graph));
        memset(earliest,0,sizeof(earliest));
        memset(latest,0,sizeof(latest));
        memset(inDegree,0,sizeof(inDegree));
        while(m--){
            int from,to,length;
            cin>>from>>to>>length;
            graph[from].push_back(Edge(to,length));
            inDegree[to]++;
        }
        CriticalPath(n);
        int answer=0;
        for(int i=0;i<n;i++){
            answer=max(answer,earliest[i]);
        }
        cout<<answer<<endl;
    }
}

```
[\[编程题\]Jungle Roads](https://www.nowcoder.com/questionTerminal/75c19b92d6b942f08989b335afbc73c3) Kruskal 最小生成树，如果有结构体可以用指针存储在 unordered_map中，最小生成树套路就按这样UF（id，item2int，int2item，parent），仿函数写
```cpp
#include<bits/stdc++.h>
using namespace std;
class edge{
public:
    char x,y;
    int weight;
    edge(char _x,char _y,int _w):x(_x),y(_y),weight(_w){};
};
class cmp{
public:
    bool operator()(const edge& e1,const edge& e2)const{
        return e1.weight>e2.weight;
    }
};
class UF{
public:
    unordered_map<char,int> item2int;
    priority_queue<edge,vector<edge>,cmp> q;
    vector<char> int2item;
    vector<int> parent;
    int id;
    int cnt;
    UF(int maxsize){
        id=0;
        int2item.resize(maxsize);
        for (int i=0;i<maxsize;i++){
            parent.push_back(-1);
        }
    }
    void insertEdge(char from,char to,int w){
        if (item2int.find(from)==item2int.end()){
            item2int[from]=id;
            int2item[id]=from;
            id++;
        }
        if (item2int.find(to)==item2int.end()){
            item2int[to]=id;
            int2item[id]=to;
            id++;
        }
        q.push(edge(from,to,w));
    }
    int find(int x){
        if (parent[x]>=0) return parent[x]=find(parent[x]);
        return x;
    }
    void Union(char x,char y){
        int root1=find(item2int[x]);
        int root2=find(item2int[y]);
        if (root1==root2) return;
        if (parent[root1]<=parent[root2]){
            parent[root1]+=parent[root2];
            parent[root2]=root1;
        }
        else{
            parent[root2]+=parent[root1];
            parent[root1]=root2;
        }
        cnt--;
    }
    bool connected(char x,char y){
        int root1=find(item2int[x]);
        int root2=find(item2int[y]);
        return root1==root2;
    }
    void kruscal(){
        /*
        for (auto x:item2int){
            cout << x.first << ' ' << x.second;
        }
        */
        //cout << endl;
        cnt=id;
        int sum=0;
        /*
        //cout << cnt << endl;
        while (!q.empty()){
            cout << q.top().weight << ' ';q.pop();
        }
        */

        while (cnt>1){
            edge e=q.top();q.pop();
            //cout << "e.x=" << e.x << " e.y=" << e.y << endl;
            if (!connected(e.x,e.y)){
                sum+=e.weight;
                Union(e.x,e.y);
            }
        }
        cout << sum << endl;
        return;
    }
};
int main(){
    int n,k,w;
    while (cin >> n){
        if (n==0) return 0;
        //cout << "n=" << n << endl;
        UF u(30);
        char c,tmp;
        for (int i=0;i<n-1;i++){
            cin >> c >> k;
            for (int j=0;j<k;j++){
                cin >> tmp >> w;
                u.insertEdge(c,tmp,w);
            }
        }
        u.kruscal();
    }
}
```
拓扑排序是AOV网络，[AOE 网络是关键活动](https://www.bilibili.com/video/BV1H4411N7oD?p=102&vd_source=6c26f427606a59575440e9bc6cec44af)，[7-11 关键活动](https://pintia.cn/problem-sets/15/exam/problems/719)注意看题目 关键活动输出的顺序规则是：任务开始的交接点编号小者优先，起点编号相同时，与输入时任务的顺序相反

```cpp
#include <bits/stdc++.h>
using namespace std;
class AOE{
public:
    vector<vector<int>> graph;
    vector<int> inDegree;   //记录每个节点的入度
    vector<int> outDegree;   //记录每个点的出度
    vector<int> Earliest;   //最早完成的时间
    vector<int> Latest;    //最短可以完成的时间
    int last=INT32_MIN;     //记录最后完成时间
    AOE(int n){
        graph.resize(n);
        Latest.resize(n);
        for (int i=0;i<n;i++) inDegree.push_back(0);
        for (int i=0;i<n;i++) Earliest.push_back(-1e5);
        for (int i=0;i<n;i++) outDegree.push_back(0);
        for (int i=0;i<n;i++){
            for (int j=0;j<n;j++) graph[i].push_back(-1); //-1表示不可达
        }
    }
    void print(){
        for (int i=0,len=graph.size();i<len;i++){
            for (int j=0;j<len;j++){
                cout << graph[i][j] << ' ';
            }
            cout << endl;
        }
    }
    void readEdge(int v1,int v2,int w){
        graph[v1][v2]=w;
        inDegree[v2]++;     //v2的入度+1
        outDegree[v1]++;    //v1的出度+1
    }
    int forward(){
        //前推得到最早完成时间,与拓扑排序类似
        //每次找到入度为0的点,更新Earliest
        //Earliest[j]=max(Earliest[i]+graph[i][j]) for all i->j
        queue<int> q;   //收录入度为0的点
        vector<bool> collected(graph.size(),false);
        for (int i=0,len=graph.size();i<len;i++){
            if (inDegree[i]==0){
                q.push(i);
                Earliest[i]=0;
                collected[i]=true;
                //cout << i << endl;
            }
        }
        while (!q.empty()){
            int x=q.front();q.pop();
            //更新Earliest[x],加!collected[x]是为了避免起点二次更新
            int maxLen=-1;
            if (!collected[x]){
                for (int i=0,len=graph.size();i<len;i++){
                    if (graph[i][x]!=-1){
                        if (Earliest[i]+graph[i][x]>maxLen){
                            maxLen=Earliest[i]+graph[i][x];
                        }
                    }
                }
                Earliest[x]=maxLen;
                collected[x]=true;
            }
            for (int i=0,len=graph.size();i<len;i++){
                if (graph[x][i]!=-1){
                    inDegree[i]--;
                    if (inDegree[i]==0) q.push(i);
                }
            }
        }
        for (int i=0,len=graph.size();i<len;i++){
            if (!collected[i]) return 0;
            else if (Earliest[i]>last){
                last=Earliest[i];
            }
        }
        return last;
    }
    void backforward(){
        //后推得到最晚完成时间,和拓扑结构对称
        //Latest[i]=min(Latest[j]-graph[i][j])  for all i->j
        queue<int> q;   //收录出度为0的点
        vector<bool> collected(graph.size(),false);
        for (int i=0,len=graph.size();i<len;i++){
            if (outDegree[i]==0){
                q.push(i);
                Latest[i]=last;
                collected[i]=true;
                //cout << i << endl;
            }
        }
        while (!q.empty()){
            int x=q.front();q.pop();
            //更新Latest[x],加!collected[x]是为了避免终点二次更新
            int minLen=1e5;
            //cout << "开始收录" << x << endl;
            if (!collected[x]){
                for (int i=0,len=graph.size();i<len;i++){
                    if (graph[x][i]!=-1){
                        if (Latest[i]-graph[x][i]<minLen){
                            minLen=Latest[i]-graph[x][i];
                            //cout << "minLen更新" << Latest[i] << "  " << graph[x][i] << endl;
                        }
                    }
                }
                Latest[x]=minLen;
                collected[x]=true;
            }
            for (int i=0,len=graph.size();i<len;i++){
                if (graph[i][x]!=-1){
                    outDegree[i]--;
                    if (outDegree[i]==0) q.push(i);
                }
            }
        }
        //for (int i=0,len=graph.size();i<len;i++) cout << Latest[i] << ' ';
    }
    void printRes(){
        //关键路径是graph[i][j]==Latest[j]-Earliest[i]
        for (int i=0,len=graph.size();i<len;i++){
            for (int j=len-1;j>=0;j--)  //这个第二个测试点可以过
            {
                if (graph[i][j]!=-1 && graph[i][j]==Latest[j]-Earliest[i]){
                    printf("\n%d->%d",i+1,j+1);
                }
            }
        }
    }
};





int main(){
    int n,m,v1,v2,w;
    scanf("%d%d",&n,&m);
    AOE aoe(n);
    for (int i=0;i<m;i++){
        scanf("%d%d%d",&v1,&v2,&w);
        aoe.readEdge(v1-1,v2-1,w);
    }
    //aoe.print();
    int last=aoe.forward();
    printf("%d",last);
    aoe.backforward();
    aoe.printRes();
}
```


[7-35 城市间紧急救援](https://pintia.cn/problem-sets/15/exam/problems/862)，自己写的回溯内存超限，[【PTA】7-35 城市间紧急救援（Dijkstra最短路）](https://blog.csdn.net/Skyed_blue/article/details/97007435?ops_request_misc=%257B%2522request%255Fid%2522%253A%2522166913174716782425685418%2522%252C%2522scm%2522%253A%252220140713.130102334..%2522%257D&request_id=166913174716782425685418&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~sobaiduend~default-1-97007435-null-null.142%5Ev66%5Econtrol,201%5Ev3%5Eadd_ask,213%5Ev2%5Et3_control2&utm_term=7-35%20%E5%9F%8E%E5%B8%82%E9%97%B4%E7%B4%A7%E6%80%A5%E6%95%91%E6%8F%B4&spm=1018.2226.3001.4187)
[7-36 社交网络图中结点的“重要性”计算](https://pintia.cn/problem-sets/15/exam/problems/863)，起初做的是用 floyd，用 BFS 复杂度更好。
```cpp
#include <bits/stdc++.h>
using namespace std;

vector<vector<int>> paths;  //存储合理路径经过的顶点
vector<int> cur;            //存储当前路径
vector<bool> collected;     //当前路径已经走过的那些点

void print(const vector<vector<int>> graph){
    for (int i=0,len=graph.size();i<len;i++){
        for (int j=0;j<graph[i].size();j++){
            cout << graph[i][j] << ' ';
        }
        cout << endl;
    }
}
void backTracking(const vector<vector<int>> graph,int start,int end){
    //从start开始到end,收录入paths
    int len=graph.size();
    if (cur.size()==len || !cur.empty() && cur.back()==end){
        paths.push_back(cur);
        return;
    }
    //每次收录一条没收录的路径
    for (int i=0;i<len;i++){
        if (graph[start][i]!=0 && !collected[i]){
            cur.push_back(i);
            collected[i]=true;
            backTracking(graph,i,end);
            cur.pop_back();
            collected[i]=false;
        }
    }
}


int main(){
    //有一张有权图,每个顶点有救援队,最短路径且召集尽可能多的救援队
    int n,m,s,d,tmp1,tmp2,w;
    scanf("%d%d%d%d",&n,&m,&s,&d);
    vector<vector<int>> graph(n,vector<int>(n,0));
    vector<int> rescuer(n,0);
    for (int i=0;i<n;i++){
        scanf("%d",&rescuer[i]);
    }
    for (int i=0;i<m;i++){
        scanf("%d%d%d",&tmp1,&tmp2,&w);
        graph[tmp1][tmp2]=w;
        graph[tmp2][tmp1]=w;
    }
    //print(graph);
    collected.resize(n);
    //for (int i=0;i<n;i++) cout << collected[i] << endl;
    backTracking(graph,s,d);
    int maxNum=0,minPath=INT32_MAX;
    int cnt=0;
    vector<int> res;
    for (int i=0,len=paths.size();i<len;i++){
        int sum=graph[s][paths[i][0]];
        for (int j=0,l=paths[i].size()-1;j<l;j++) sum+=graph[paths[i][j]][paths[i][j+1]];
        //cout << sum << endl;
        //for (int j=0,len=paths[i].size();j<len;j++) cout << paths[i][j] << ' ';
        //cout << endl;
        if (sum<minPath){   //保证最短路径
            minPath=sum;
            cnt=1;
            res=paths[i];
            int num=rescuer[s];
            for (int j=0,l=paths[i].size();j<l;j++) num+=rescuer[paths[i][j]];
            maxNum=num;
        }
        else if (sum==minPath){
            cnt++;
            int num=rescuer[s];
            for (int j=0,l=paths[i].size();j<l;j++) num+=rescuer[paths[i][j]];
            if (num>maxNum){
                maxNum=num;
                res=paths[i];
            }
        }
    }
    cout << cnt << ' ' << maxNum << endl << s;
    for (int i=0,len=res.size();i<len;i++) cout << ' ' << res[i];
    return 0;
}
```
[7-9 旅游规划](https://pintia.cn/problem-sets/15/problems/717) 同时优化多个变量，抓住程序中的更新步骤即可。不记得 Dijkstra 就写 Floyd，单源最短路也可以用多源的解决
```cpp
#include <iostream>
#include <vector>
#define INF 1000000
using namespace std;

class info{ //不能用data命名,会报错
public:
    int len;
    int charge;
    info(int l,int c):len(l),charge(c){};
    info():len(INF),charge(INF){};
};

void print(vector<vector<info>>& g){
    for (int i=0;i<g.size();i++){
        for (int j=0;j<g.size();j++){
            cout << g[i][j].len << ' ';
        }
        cout << endl;
    }
    cout << "--------------------" << endl;
    for (int i=0;i<g.size();i++){
        for (int j=0;j<g.size();j++){
            cout << g[i][j].charge << ' ';
        }
        cout << endl;
    }
}

int main(){
    int n,m,s,d,v1,v2;
    cin >> n >> m >> s >> d;
    vector<vector<info>> graph(n,vector<info>(n));
    for (int i=0;i<m;i++){
        cin >> v1 >> v2;
        cin >> graph[v1][v2].len >> graph[v1][v2].charge;
        graph[v2][v1]=graph[v1][v2];
    }
    //print(graph);

    vector<vector<info>> dis(n,vector<info>(n)); //保证len最小的前提下尽量减小charge
    //初始化dis
    for (int i=0;i<n;i++){
        for (int j=0;j<n;j++){
            if (graph[i][j].len!=INF){
                dis[i][j]=graph[i][j];
            }
        }
    }
    //print(dis);
    //floyd
    for (int k=0;k<n;k++){
        for (int i=0;i<n;i++){
            for (int j=0;j<n;j++){ //更小的len就更新len,不管charge怎么样
                if (dis[i][j].len>dis[i][k].len+dis[k][j].len){
                    dis[i][j].len=dis[i][k].len+dis[k][j].len;
                    dis[i][j].charge=dis[i][k].charge+dis[k][j].charge;
                }
                else if ((dis[i][j].len==dis[i][k].len+dis[k][j].len)
                        && dis[i][j].charge>dis[i][k].charge+dis[k][j].charge){
                    //如果距离一样就减小charge
                    dis[i][j].charge=dis[i][k].charge+dis[k][j].charge;
                }
            }
        }
    }

    cout << dis[s][d].len <<  ' ' << dis[s][d].charge;
}
/*
#include <iostream>
#include <vector>
#define INF 1000000
using namespace std;

class info{
public:
    int len;
    int charge;
    info(int l,int c):len(l),charge(c){};
    info():len(INF),charge(INF){};
};

void print(vector<vector<info>>& g){
    for (int i=0;i<g.size();i++){
        for (int j=0;j<g.size();j++){
            cout << g[i][j].len << ' ';
        }
        cout << endl;
    }
    cout << "--------------------" << endl;
    for (int i=0;i<g.size();i++){
        for (int j=0;j<g.size();j++){
            cout << g[i][j].charge << ' ';
        }
        cout << endl;
    }
}

int main(){
    int n,m,s,d,v1,v2;
    cin >> n >> m >> s >> d;
    vector<vector<info>> graph(n,vector<info>(n));
    for (int i=0;i<m;i++){
        cin >> v1 >> v2;
        cin >> graph[v1][v2].len >> graph[v1][v2].charge;
        graph[v2][v1]=graph[v1][v2];
    }
    //print(graph);

    vector<info> dis(n); //保证len最小的前提下尽量减小charge
    //初始化dis
    for (int i=0;i<n;i++){
        if (graph[s][i].len!=INF) dis[i]=graph[s][i];
    }

    //dijkstra
    vector<bool> collected(n,false);
    dis[s].len=0;dis[s].charge=0;
    while (true){
        //每次取出一个最优的点收录
        info opt;
        int v=-1;
        for (int i=0;i<n;i++){
            if (!collected[i]){
                if (dis[i].len<opt.len){
                    v=i;
                    opt=dis[i];
                }
                else if ((dis[i].len==opt.len) && (dis[i].charge<opt.charge)){
                    v=i;
                    opt=dis[i];
                }
            }
        }
        if (v==-1) break;
        collected[v]=true;
        for (int w=0;w<n;w++){
            //更新和v连着的点
            if (!collected[w]){
                if (dis[w].len>graph[v][w].len+dis[v].len){
                    dis[w].len=graph[v][w].len+dis[v].len;
                    dis[w].charge=graph[v][w].charge+dis[v].charge;
                }
                else if ((dis[w].len==graph[v][w].len+dis[v].len) && (dis[w].charge>graph[v][w].charge+dis[v].charge)){
                    dis[w].charge=graph[v][w].charge+dis[v].charge;
                }
            }
        }
    }
    cout << dis[d].len << ' ' << dis[d].charge;
}
*/
```
[7-10 公路村村通](https://pintia.cn/problem-sets/15/problems/718) Prim 记录 cost

```cpp
#include <iostream>
#include <vector>
#define INF 1000000
using namespace std;

int main(){
    int n,m,v1,v2,w;
    cin >> n >> m;
    vector<vector<int>> graph(n,vector<int>(n,INF));
    for (int i=0;i<m;i++){
        cin >> v1 >> v2 >> w;
        graph[v1-1][v2-1]=w;
        graph[v2-1][v1-1]=w;
    }
    vector<int> dis(n,INF);
    vector<int> parent(n,INF);
    parent[0]=-1;
    for (int i=0;i<n;i++){
        if (graph[0][i]!=INF){
            dis[i]=graph[0][i];
            parent[i]=0;
        }
    }
    dis[0]=0;
    int cnt=1;
    int cost=0;
    while (true){
        int v=-1,minDis=INF;
        for (int i=0;i<n;i++){
            if (dis[i]!=0 && dis[i]<minDis){
                v=i;
                minDis=dis[i];
            }
        }
        if (v==-1) break;
        dis[v]=0;cnt++;
        cost+=graph[parent[v]][v]; //树长大了记录cost
        for (int w=0;w<n;w++){
            if (graph[v][w]!=INF && dis[w]!=0 && dis[w]>graph[v][w]+dis[v]){
                dis[w]=graph[v][w]+dis[v];
                parent[w]=v;
            }
        }
    }
    if (cnt<n) cout << "-1";
    else cout << cost;
}
```

```cpp
#include <iostream>
#include <queue>
#define MaxSize 100
#define INF 10000  //不能用INT32_MAX,相加的时候会溢出变成负数,最短路径初始化权重必须大于路径之和,而不能仅仅大于单条
using namespace std;


template<typename weightType>
class mEdge{
public:
    int v1,v2;
    weightType weight;
    mEdge(int _v1,int _v2,weightType _weight):v1(_v1),v2(_v2),weight(_weight){};
    bool operator < (const mEdge& edge) const{
        return weight>edge.weight; //此对象的weight大于另一个就返回true;当e1.w>e2.w的时候,(e1<e2)->true
    }
};

class UF{
public:
    int parent[MaxSize];
    UF(int num){
        for (int i=0;i<num;i++) parent[i]=-1;
    }
    int Find(int x){
        if (parent[x]>=0){
            return parent[x]=Find(parent[x]);
        }
        else return x;
    }
    void Union(int x1,int x2){
        int root1=Find(x1);
        int root2=Find(x2);
        if (parent[root1]<parent[root2]) parent[root2]=root1; //root1是大树,root2挂到root1下面
        else if (parent[root1]>parent[root2]) parent[root1]=root2;
        else{
            parent[root2]=root1;
            parent[root1]--;
        }
    }
    bool connected(int x1,int x2){
        int root1=Find(x1);
        int root2=Find(x2);
        return root1==root2;
    }
};

template<typename weightType,typename dataType>
class mGraph{
public:
    //邻接矩阵表示的有权图
    //图是一种多对多关系,是链表和树的扩展
    //(v,w)是无向边,<v,w>是有向边
    //图的数据包括非空有限顶点集和有限边集
    //{G_00,G_10,G_11,...,G_n-1n-1}可以省一半空间,Gij的下标是(i*(i+1)/2+j)
    //☆有权图如果没有边应该怎么表示?☆
    int nV,nE; //边数和顶点数
    weightType weightedG[MaxSize][MaxSize]={0};   //带权重的图
    dataType data[MaxSize]={0};                  //存每个顶点的数据
    bool visited[MaxSize]={false};             //遍历图需要
    mGraph(int vertexNum):nE(0),nV(vertexNum){};
    void InsertEdge(mEdge<weightType>* e){
        if (e->v1>=nV && e->v2>=nV){
            cout << "边的顶点违法" << endl;
            return;
        }
        weightedG[e->v1][e->v2]=e->weight;
        nE++;
    }
    void DFS(int v){
        //O(N^2)
        cout << v << ' ';
        visited[v]=true;
        for (int i=0;i<nV;i++){
            if (weightedG[v][i]!=(weightType)0 && !visited[i]) DFS(i);
        }
    }
    void reSetVisited(){
        //遍历结束之后需要重置visited方便下次遍历
        for (int i=0;i<nV;i++) visited[i]=false;
    }
    void BFS(int v){
        //O(N^2)
        queue<int> q;
        visited[v]=true;
        q.push(v);
        cout << v << ' ';
        while (!q.empty()){
            int w=q.front();q.pop();
            for (int i=0;i<nV;i++){
                if (weightedG[w][i]!=(weightType)0 && !visited[i]){
                    cout << i << ' ';
                    visited[i]=true;
                    q.push(i);
                }
            }
        }
    }
    void unWeightedShortPath(int start){
        //从start到其他点的最短路径,O(V+E)
        int dis[nV];
        for (int i=0;i<nV;i++) dis[i]=INF;
        int path[nV];
        //类似BFS
        queue<int> q;
        dis[start]=0;
        q.push(start);
        while (!q.empty()){
            int v=q.front();q.pop();
            for (int w=0;w<nV;w++){
                if (weightedG[v][w]==1 && dis[w]==INF){
                    dis[w]=dis[v]+1;
                    path[w]=v;          //start到w的必经顶点就是前一个顶点v,v->w
                }
            }
        }
        //遍历路径可以用堆栈处理path
    }
    //这里忘了☆☆☆☆☆☆☆☆☆☆
    //☆☆☆☆☆☆☆☆☆☆☆☆☆☆
    void Dijkstra(int start){
        int path[nV],dis[nV];
        bool collected[nV]={false};
        for (int i=0;i<nV;i++){
            if (weightedG[start][i]!=0){
                dis[i]=weightedG[start][i];
            }
            else dis[i]=INF;
        }
        dis[start]=0;
        //不需要collected[0]=true,下面的wihile会收录
        //找到未收录顶点中距离最小的
        //全扫描O(V^2+E),最小堆O(ElogV)
        //这里用while,不能用for,万一图不连通就完了
        //☆☆☆☆☆☆☆☆☆☆☆☆☆☆☆☆
        while (true){
            int v=-1,minDis=INF;
            for (int i=0;i<nV;i++){
                if (!collected[i]){
                    if (minDis>dis[i]){
                        minDis=dis[i];
                        v=i;
                    }
                }
            }
            if (v==-1) return;
            collected[v]=true;
            //收录v之后把与v连接的那些点更新一下
            for (int w=0;w<nV;w++){
                if (!collected[w] && weightedG[v][w]!=0){
                    if (weightedG[v][w]+dis[v]<dis[w]){
                        dis[w]=weightedG[v][w]+dis[v];
                        path[w]=v;
                    }
                }
            }
        }
    }
    //floyd可以处理有负边的最短路径,但无法处理有负值回路的情况
    void Floyd(){
        //稠密图更合算O(N^3)
        int dis[nV][nV],path[nV][nV];
        for (int i=0;i<nV;i++){
            for (int j=0;j<nV;j++){
                if (weightedG[i][j]!=0){
                    dis[i][j]=weightedG[i][j];
                }
                else if (i==j) dis[i][j]=0;	//注意这里一定是0
                else dis[i][j]=INF;
            }
        }
        for (int k=0;k<nV;k++){
            //从i-j只经过小于k的那些点
            for (int i=0;i<nV;i++){
                for (int j=0;j<nV;j++){
                    if (dis[i][k]+dis[k][j]<dis[i][j]){
                        dis[i][j]=dis[i][k]+dis[k][j];
                        path[i][j]=k;
                    }
                }
            }
        }
    }
    //来自《数据结构与算法分析》
    void SPFA(int start){
        queue<int> q;
        int dis[nV],path[nV],cnt[nV];  //cnt计数每个顶点出队次数,如果出队次数达到nV+1则判断有回路
        for (int i=0;i<nV;i++){
            dis[i]=INF;
            cnt[i]=0;
        }
        dis[start]=0;
        bool inQ[nV]={false};
        q.push(start);
        while (!q.empty()){
            int v=q.front();q.pop();
            cnt[v]++;
            if (cnt[v]>nV){
                cout << "有负值回路" << endl;
                return;
            }
            for (int w=0;w<nV;w++){
                if (weightedG[v][w]!=INF && v!=w){  //这里目的是判断v和w是否邻接,视初始情况写判定条件
                    if (dis[w]>dis[v]+weightedG[v][w]){
                        dis[w]=dis[v]+weightedG[v][w];
                        path[w]=v;
                        if (!inQ[w]){
                            q.push(w); //能找到更短的路径就有松弛的机会
                            inQ[w]=true;
                        }
                    }
                }

            }
        }
    }
    //忘记了☆☆☆☆☆☆☆☆☆☆
    //☆☆☆☆☆☆☆☆☆☆☆☆☆
    bool Prim(int start){
        //生成树是指包含所有顶点,且nV-1条边图内都存在
        //最小生成树算法,正好用掉nV-1条权重和最小的边使使图连通
        //慢慢把树长大,类似Dijkstra
        //dis[v]==0表示v已经被收录入最小生成树
        //最后图不连通的话生成树不存在
        //O(V^2),适合用于稠密图
        //下面的实现默认图没有环
        int parent[nV]; //节点i的父节点是parent[i]
        for (int i=0;i<nV;i++) parent[i]=INF;
        parent[start]=-1; //根节点
        int dis[nV];    //dis[i]表示节点i离最小生成树的距离
        dis[start]=0;
        int collected=1; //记录最小生成树的顶点数
        for (int i=0;i<nV;i++){
            if (weightedG[start][i]!=0){
                dis[i]=weightedG[start][i];
                parent[i]=start;
            }
            else dis[i]=INF;
        }
        while (true){
            //从未收录顶点中找最小的
            int minDis=INF;
            int v=-1;
            for (int i=0;i<nV;i++){
                if (dis[i]!=0 && dis[i]<minDis){
                    minDis=dis[i];
                    v=i;
                }
            }
            if (v==-1) break;
            dis[v]=0; //收录这个顶点
            collected++;
            for (int w=0;w<nV;w++){
                if (weightedG[v][w]!=0 && dis[w]!=0 && dis[v]+weightedG[v][w]<dis[w]){  //对每个邻接点且没有收录且离最小生成树更小
                    dis[w]=weightedG[v][w];
                    parent[w]=v;
                }
            }
        }
        if (collected<nV){
            cout << "图不连通" << endl;
            return false;
        }
        return true;
    }
    bool Kruskal(){
        //把森林合并成树
        //如果用并查集和最小堆实现O(ElogE)
        priority_queue<mEdge<weightType>> q;  //默认最大堆,即if(e1<e2)的话e2先弹出,自定义<是e1.w>e2.w,当e1的权重更大时,返回e1<e2为true,此时e2先弹出
        //初始化边集合,建树的时候做,这里省略,假设已经建好
        int collected=0; //边的数量
        UF* uf=new UF(nV);
        while (collected<nV-1 && !q.empty()){
            mEdge<weightType> e=q.top();q.pop();
            if (!uf->connected(e.v1,e.v2)){  //之前两个点不连接,如果已经连接再加入这条边就会形成回路
                uf->Union(e.v1,e.v2);
                collected++;
            }
        }
        if (collected!=nV-1){
            cout << "图不连通" << endl;
            return false;
        }
        return true;
    }
    //拓扑排序,一门课是一个顶点,预备课程是边
    //找没有预备课的作为起点,AOV网络Activity On vertex
    //拓扑序是存在V->W的有向边,存在AOV就是有向无环图
    //给定DAG,输出课程排序
    bool TopSort(){
        //可以用来检测图是否存在环
        queue<int> q; //记录所有度为0的
        int cnt=0;
        for (int i=0;i<nV;i++){
            //扫描所有点的话O(V^2)
            //将入度为0的顶点放到容器比如队列内,O(V+E)
            //假设data[i]记录顶点i的入度
            //找到入度为0的顶点
            if (data[i]==0) q.push(i);
        }
        while (!q.empty()){
            int v=q.front();q.pop();
            cout << v << ' ';
            cnt++;
            for (int i=0;i<nV;i++){
                if (weightedG[v][i]!=0){
                    data[i]--; //邻接点入度减去0
                    if (data[i]==0) q.push(i);
                }
            }
        }
        if (cnt!=nV){
            cout << "图有环" << endl;
            return false;
        }
        return true;
    }
    //关键路径中☆边表示一个工程☆,顶点表示工程完成
    //计算工期前向传播取最大
    //计算机动时间反向传播取最小

};


int main(){

    return 0;
}

```

