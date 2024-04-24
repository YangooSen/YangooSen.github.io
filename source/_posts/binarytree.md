---
title: binarytree
katex: true
date: 2024-04-22 21:23:23
excerpt: 二叉树和相关应用题目
tag: ds
---

[7-23 还原二叉树](https://pintia.cn/problem-sets/15/exam/problems/838) 中，Pstart 可能 大于 Pend，因为 len 可能小于 0，因为 rootIndex 可能 等于 Istart ？

```cpp
#include <bits/stdc++.h>
using namespace std;

vector<char> preOrder;
vector<char> inOrder;
int n;
class tree{
public:
    int height;
    char data;
    tree* left;
    tree* right;
    tree(char d,int h=1,tree* l=nullptr,tree* r=nullptr):data(d),height(h),left(l),right(r){};
};

void read(){
    char c;
    cin >> n;
    for (int i=0;i<n;i++){
        cin >> c;
        preOrder.push_back(c);
    }
    for (int i=0;i<n;i++){
        cin >> c;
        inOrder.push_back(c);
    }
}
void print(const vector<char>& v,int s,int e){
    for (int i=s;i<=e;i++) cout << v[i];
    cout << endl;
}

tree* create(int Pstart,int Pend,int Istart,int Iend){
    //判断是否只有一个节点
    if (Pstart>Pend) return nullptr;
    //找到创造左右子树的两个集合
    //先序遍历第一个是树根
    char rootData=preOrder[Pstart];
    //再根据树根分割中序遍历集合
    int rootIndex=Istart;
    for (;rootIndex<=Iend && inOrder[rootIndex]!=rootData;rootIndex++);
    //[Istart,rootIndex-1]就是左子树的中序集合,下面找左子树的中序集合
    //只需根据大小判断即可
    int len=rootIndex-1-Istart;
    //cout << "左子树前序集合:";print(preOrder,Pstart+1,Pstart+len+1);
    //cout << "左子树中序集合:";print(inOrder,Istart,rootIndex-1);
    tree* left=create(Pstart+1,Pstart+len+1,Istart,rootIndex-1);
    tree* right=create(Pstart+len+2,Pend,rootIndex+1,Iend);
    int l=0,r=0;
    if (left) l=left->height;
    if (right) r=right->height;
    int h=l>r?l:r;
    return new tree(rootData,h+1,left,right);
}

int getHeight(tree* t){
    if (!t) return 0;
    int l=getHeight(t->left);
    int r=getHeight(t->right);
    return (l>r?l:r)+1;
}

int main(){
    read();
    tree* t=create(0,n-1,0,n-1);
    //cout << getHeight(t);
    cout << t->height;
}
*/
```
[1020 Tree Traversals](https://pintia.cn/problem-sets/994805342720868352/exam/problems/994805485033603072)还原二叉树首先找到中序遍历的左右集合，再**根据长度**找另一个集合，返回条件是>，不会出现只剩一个元素start==end
```cpp
#include<bits/stdc++.h>
using namespace std;
vector<int> post;
vector<int> in;
vector<int> res;
struct tree{
    tree* left;
    tree* right;
    int data;
    tree(int d,tree* l=nullptr,tree* r=nullptr):data(d),left(l),right(r){};
    void inorder(tree* t){
        if (t->left) inorder(t->left);
        cout << t->data << ' ';
        if (t->right) inorder(t->right);
    }
    void level(){
        queue<tree*> q;
        q.push(this);
        while (!q.empty()){
            tree* t=q.front();q.pop();
            res.push_back(t->data);
            if (t->left) q.push(t->left);
            if (t->right) q.push(t->right);
        }
    }
};

tree* create(int postStart,int postEnd,int inStart,int inEnd){
    if (postStart>postEnd || inStart>inEnd) return nullptr;
    tree* root=new tree(post[postEnd]);
    //cout << root->data << endl;
    int i=inStart;
    while (in[i]!=root->data){i++;}
    int len=i-inStart;
    //cout << len;
    //cout << in[i+1] << ' ' << in[inEnd] << endl;
    //cout << post[postStart+len] << ' ' << post[postEnd-1] << endl;
    root->left=create(postStart,postStart+len-1,inStart,i-1);
    root->right=create(postStart+len,postEnd-1,i+1,inEnd);
    return root;
}


int main(){
    int n;
    scanf("%d",&n);
    post.resize(n);
    in.resize(n);
    for (int i=0;i<n;i++) scanf("%d",&post[i]);
    for (int i=0;i<n;i++) scanf("%d",&in[i]);
    tree* root=create(0,post.size()-1,0,in.size()-1);
    root->level();
    cout << res[0];
    for (int i=1,len=res.size();i<len;i++) cout << ' ' << res[i];
}
```
[二叉树遍历](https://www.nowcoder.com/questionTerminal/4b91205483694f449f94c179883c1fef)，仅根据带标记的前序遍历还原普通二叉树，一个满二叉树可以根据先序和后序唯一确定
[7-30 目录树](https://pintia.cn/problem-sets/15/problems/857) ，参考 [5-30 目录树 (30分)](https://blog.csdn.net/Changxing898/article/details/52367514)
[7-27 家谱处理](https://pintia.cn/problem-sets/15/exam/problems/842)，下面自己用多叉树处理，查询操作很麻烦，测试点2 3超时，参考[PTA 7-27 家谱处理 (30分)](https://blog.csdn.net/BigDream123/article/details/104161366?ops_request_misc=%257B%2522request%255Fid%2522%253A%2522166986856416800215075804%2522%252C%2522scm%2522%253A%252220140713.130102334..%2522%257D&request_id=166986856416800215075804&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~sobaiduend~default-2-104161366-null-null.142%5Ev67%5Econtrol,201%5Ev3%5Eadd_ask,213%5Ev2%5Et3_control2&utm_term=7-27%20%E5%AE%B6%E8%B0%B1%E5%A4%84%E7%90%86&spm=1018.2226.3001.4187)（这个双亲表示法 Node 不太合适，存储的是父亲的名字，而不是父亲的位置，找祖先的时候需要先遍历找到父亲的位置，效率比较低），**用顺序结构存储的双亲表示法**不会超时，遍历的时候是自底向上遍历的，纵向上升速度很快，当孩子很多的时候，自顶向下遍历会访问到很多孩子，横向遍历浪费了很多时间。[找出直系亲属](https://www.nowcoder.com/questionTerminal/2c958d09d29f46798696f15ae7c9703b) 是 [7-27 家谱处理](https://pintia.cn/problem-sets/15/exam/problems/842) 的简化版，但需要同时找是否是两个祖先中的一个，可以用递归处理，**☆☆☆☆☆☆☆寻找两个节点的关系用顺序存储的双亲表示法☆☆☆☆☆☆☆☆**
```cpp
#include <bits/stdc++.h>
using namespace std;
class tree{
public:
    //下面的结构是关键
    struct node{
        //这里name是不必要的,但是为了适配这类题目,表示这里可以存储信息而设置name
        //当不能通过name直接访问下标的时候就需要string来设置name
        char name;
        //孩子可以放在这里,但是查找关系的时候通过孩子找双亲更快
        //vector<int> children;
        int p1,p2;    //双亲的位置
    };
    vector<node> data;
    tree(int maxsize){
        data.resize(maxsize);
        for (int i=0;i<maxsize;i++){
            data[i].name='A'+i;
            data[i].p1=-1;
            data[i].p2=-1;
        }
    }




    void setParent(const char& child,const char& parent,int mode){
        //将child的p1设为parent
        int p=0;
        while (data[p].name!=parent){p++;}
        int c=0;
        while (data[c].name!=child){c++;}
        if (mode==1) data[c].p1=p;
        else if (mode==2) data[c].p2=p;
    }
    int findParent(const char& child,const char& parent){
        int c=0;
        while (data[c].name!=child){c++;}
        int p=0;
        while (data[p].name!=parent){p++;}
        return find(c,p,0);
    }

    int find(int from, int to, int depth){
        //从from出发先序遍历到找到to为止，并返回to相对于from的深度
        if(from==to) return depth;
        if(data[from].p1!=-1)
        {
            int ret=find(data[from].p1, to, depth+1);
            if(ret!=-1) return ret;
        }
        if(data[from].p2!=-1)
        {
            int ret=find(data[from].p2, to, depth+1);
            if(ret!=-1) return ret;
        }
        return -1;
    }
};
int main(){
    int n, m;
    while(scanf("%d %d", &n, &m)!=EOF){
        tree t(27);
        while(n--)//构建树
        {
            char str[4];
            scanf("%s", str);
            if(str[1]!='-') t.setParent(str[0],str[1],1);
            if(str[2]!='-') t.setParent(str[0],str[2],2);
        }

        while(m--)//查询
        {
            char str[3];
            scanf("%s", str);
            int ans1=t.findParent(str[0],str[1]);
            if(ans1==1) printf("child\n");
            else if(ans1>=2)
            {
                for(int i=ans1; i>2; i--) printf("great-");
                printf("grandchild\n");
            }
            else//看来不是晚辈，那就是长辈了
            {
                int ans2=t.findParent(str[1],str[0]);
                if(ans2==1) printf("parent\n");
                else if(ans2>=2)
                {
                    for(int i=ans2; i>2; i--) printf("great-");
                    printf("grandparent\n");
                }
                else printf("-\n");//也不是长辈，那就不是直系亲属
            }
        }
    }
    return 0;//大功告成
}

```
```cpp
#include <bits/stdc++.h>
using namespace std;
class levelAndName{
public:
    int level;  //计算树的层数,k/2
    string name;
    levelAndName(int l,string n):level(l),name(n){};
};
class tree{
public:
    string name;
    vector<tree*> children;
    tree(string n):name(n){};
    //层序遍历
    void levelTraversal(){
        queue<tree*> q;
        q.push(this);
        while (!q.empty()){
            tree* cur=q.front();q.pop();
            cout << cur->name << ' ';
            for (auto x:cur->children) q.push(x);
        }
    }
    tree* find(const string& name){
        queue<tree*> q;
        q.push(this);
        while (!q.empty()){
            tree* cur=q.front();q.pop();
            if (cur->name==name) return cur;
            for (auto x:cur->children) q.push(x);
        }
        return nullptr;
    }
    tree* findFather(const string& name){
        queue<tree*> q;
        q.push(this);
        while (!q.empty()){
            tree* cur=q.front();q.pop();
            if (cur->name==name) return cur;
            for (auto x:cur->children){
                if (x->name==name) return cur;
                q.push(x);
            }
        }
        return nullptr;
    }
};

vector<levelAndName> v;

tree* createTree(int s,int e){
    //根据[s,e]创建子树,返回根节点
    if (s==e) return new tree(v[s].name);
    //for (int i=s;i<=e;i++) cout << v[i].name << ' ' << v[i].level << endl;
    tree* root=new tree(v[s].name);
    int childLevel=v[s].level+1;
    int childS=s+1,i;
    for (i=childS+1;i<=e;i++){
        if (v[i].level==childLevel){
            //cout << v[childS].name << ' ' << v[i-1].name << endl;
            root->children.push_back(createTree(childS,i-1));
            childS=i;
        }
    }
    //cout << v[childS].name << ' ' << v[i-1].name << endl;
    root->children.push_back(createTree(childS,i-1));
    return root;
}
int main(){
    int n,m;
    scanf("%d%d",&n,&m);
    string x,y,relation,_;
    char c;
    getchar();
    for (int i=0;i<n;i++){
        //统计空格数目
        int cnt=0;
        string name="";
        c=getchar();
        while (c!='\n'){
            if (c==' ') cnt++;
            else name.push_back(c);
            c=getchar();
        }
        //cout << cnt << ' ' << name << endl;
        levelAndName ii(cnt/2,name);
        v.push_back(ii);
    }

    tree* root=createTree(0,n-1);
    //root->levelTraversal();

    for (int i=0;i<m;i++){
        cin >> x >> _ >> _ >> relation >> _ >> y;
        bool flag=false;
        if (relation=="child" || relation=="parent"){
            if (relation=="parent") swap(x,y);
            //先找到y,再看看x在不在y的children里
            tree* yy=root->find(y);
            for (auto xx:yy->children){
                if (x==xx->name){
                    printf("True\n");
                    flag=true;
                    break;
                }
            }
            if (!flag) printf("False\n");
        }
        else if (relation=="sibling"){
            //找到x的父节点,判断y是否在里面
            //cout << x << endl;
            tree* xx=root->findFather(x);
            //cout << xx->name << endl;
            for (auto yy:xx->children){
                if (y==yy->name){
                    printf("True\n");
                    flag=true;
                    break;
                }
            }
            if (!flag) printf("False\n");
        }
        if (relation=="ancestor" || relation=="descendant"){
            if (relation=="descendant") swap(x,y);
            //cout << x << ' ' << y << endl;
            //判断x是否是y的祖先
            //不断找y的father的father,判断是否是x
            tree* cur=nullptr;
            while (cur!=root){
                cur=root->findFather(y);
                if (cur->name==x){
                    printf("True\n");
                    flag=true;
                    break;
                }
            }
            if (!flag) printf("False\n");
        }
    }

}
```
判断两颗二叉树是否一样，下面的第一种可以，第二种不行
```cpp
bool isSame(tree* r1,tree* r2){
    if (!r1 && !r2) return true;
    if (r1->c!=r2->c) return false;
    return isSame(r1->left,r2->left)&&isSame(r1->right,r2->right);
}
/*
bool isSame(tree* r1,tree* r2){
    if (!isSame(r1->left,r2->left)) return false;
    if (r1->c!=r2->c) return false;
    if (!isSame(r1->right,r2->right)) return false;
    return true;
}
*/
```
```cpp
#include<iostream>
#include<stack>
#include<queue>
using namespace std;
#define MAXSIZE 100
template<typename T>
class sequentialTree{
    T data[MAXSIZE]; //父节点i/2,左孩子2i,右孩子2i+1,没有儿子留空
};

template<typename T>
class treeNode{         //△树有一个节点和m个<互不相交>的有限集合,每个集合是一个树△

    treeNode<T>* left;     //树是保证节点连接的最少边的结构
    treeNode<T>* right;    //树的度是节点最大的度
    T data;         //二叉树第i层最多有2^{i-1}个节点,深度为k的二叉树总结点最多2^{k}-1
                    //n_0=n_2+1,从边的视角看,除根节点每个节点对应一条边n_0+n_1+n_2-1=0*n_0+1*n_1+2*n2
    void preOrderTraversal(treeNode<T>* bt){
        //递归实现
        if (bt){
            cout << bt->data << ' ';        //每个元素都访问了三次,区别只是三次中的哪次输出
            preOrderTraversal(bt->left);
            preOrderTraversal(bt->right);
        }
    }
    void postOrderTraversal(){ //非递归不需要参数
        //非递归实现
        stack<treeNode<T>*> st;
        treeNode<T>* node=this;
        st.push(node);
        while (!st.empty()){
            node=st.top();st.pop();
            if (!node){
                //要遍历节点后面放一个空标记,左右中,压栈顺序是中右左
                //不管是哪种遍历,放完中节点之后加一个空标记
                st.push(node);
                st.push(nullptr);

                if (node->right) st.push(node->right);
                if (node->left) st.push(node->left);

            }
            else{
                node=st.top();st.pop();
                //myFunction(node->data); //处理节点
                cout << node->data << ' ';
            }
        }
    }
    void levelTraversal(){
        queue<treeNode<T>*> q;
        treeNode<T>* node=this;
        q.push(node);
        while (!q.empty()){
            node=q.front();q.pop();
            cout << node->data << ' ';
            if (node->left) q.push(node->left);
            if (node->right) q.push(node->right);
        }
    }
    int getHeight(treeNode<T>* bt){
        if (bt){
            int hL=getHeight(bt->left);
            int hR=getHeight(bt->right);
            return (hL>hR)?hL+1:hR+1;
        }
        return 0; //空树高度是1,也就是到了叶子节点,然后叶子节点的父节点就会得到高度0+1
    }
};
template<typename T>
class binarySearchTreeNode{
    binarySearchTreeNode<T>* left;
    binarySearchTreeNode<T>* right;
    T data;
    binarySearchTreeNode(T x):data(x),left(nullptr),right(nullptr){};
    binarySearchTreeNode<T>* find(T x){
        binarySearchTreeNode<T>* node=this;
        while (node){ //△注意这里的实现△
            if (x>node->data) node=node->right;
            if (x<node->left) node=node->left;
            else return node;
        }
        return nullptr;
    }
    binarySearchTreeNode<T>* findMin(binarySearchTreeNode<T>* node){
        if (node){
            while (node->left) node=node->left; //△注意这里不能用while(node),不然最后返回空△
        }
        return node;
    }
    binarySearchTreeNode<T>* insert(T x,binarySearchTreeNode<T>* node){
        //insert的功能是将x插入node这棵树中,并且返回插入后的新树,基本的树node已经给了,如果重新定义node就不是插入后的新树了
        //即不能 if (!node) AVLTree<T>* node=new binarySearchTreeNode<T>(x);
        if (!node){
           node=new binarySearchTreeNode<T>(x); //这里重定义node,不能用其他命名,为了始终return node
        }
        else{ //注意这里不能再用if或else if判断,此时node已经变了
            if (x<node->left) node->left=insert(x,node->left);
            else if (x>node->right) node->right=insert(x,node->right);
        }
        return node; //△注意插入实现,别用尾递归return一个函数,永远返回node就可以△
    }
    binarySearchTreeNode<T>* Delete(T x,binarySearchTreeNode<T>* node){
        binarySearchTreeNode<T>* tmp;
        if (!node) cout << "要删除的元素未找到" << endl;
        if (x<node->data) node->left=Delete(x,node->left);
        else if (x>node->data) node->right=Delete(x,node->right);
        else{
            //找到了要删除的节点
            if (node->right && node->left){
                //有两个孩子,用右子树最小值或左子树最大值代替当前节点的值,然后删掉那个节点
                tmp=findMin(node->right);
                node->data=tmp->data;
                node->right=Delete(tmp->data,node->right);
            }
            else{
                tmp=node; //保存当前节点之后删除
                if (node->right) node=node->right;
                else if (node->left) node=node->left; //用子节点覆盖当前节点
                delete tmp; //放在这里没问题,因为上面那个有两个孩子节点的最后也会落到这步
            }
        }
        return node; //△永远返回node△
    }
};


//△平衡二叉树的实现,注意插入和高度的实现,难点!!!△
template<typename T>
class AVLTree{
    public:
    AVLTree* left;
    AVLTree* right;
    int height;
    T data;
    AVLTree(T x):left(nullptr),right(nullptr),data(x),height(0){}; //注意只有一个节点的数高度是0

    int getHeight(AVLTree<T>* A){
        return A==nullptr?-1:A->height;
    }
    int reSetHeight(AVLTree<T>* A){
        return max(getHeight(A->left),getHeight(A->right)) + 1;
    }

    AVLTree<T>* RRRotation(AVLTree<T>* A){
        //A的平衡被破坏,它的右子树的右子树(C)上有新插入的节点,要进行右右单旋
        /* A           B
             B  ->   A   C
              C
         */
        //把A的右节点变成根,把A挂到B的左边,但之前需要把B的左边空出来,此时已经拿掉A的右子树(B)了,因此可以挂在A右边
        AVLTree<T>* B=A->right;
        A->right=B->left;
        B->left=A;
        A->height=reSetHeight(A);
        B->height=reSetHeight(B);
        return B;
    }
    AVLTree<T>* LLRotation(AVLTree<T>* A){
        //A的平衡被破坏,它的左子树的左子树(C)上有新插入的节点,要进行左左单旋
        /* A           B
         B      ->   C   A
       C
         */
        //把A的左节点变成根,把A挂到B的右边,但之前需要把B的右边空出来,此时已经拿掉A的左子树(B)了,因此可以挂在A左边
        AVLTree<T>* B=A->left;
        A->left=B->right;
        B->right=A;
        A->height=reSetHeight(A);
        B->height=reSetHeight(B);       //别忘了重设树高
        return B;

    }
    AVLTree<T>* LRRotation(AVLTree<T>* A){
        //A的平衡被破坏,它的左子树的右子树(C)上有新插入的节点,要进行左右双旋
        /* A          A           C
         B    ->    C     ->    B   A
           C      B
         */
        //先变成左左旋转的格式,需要右旋B ☆左右双旋先右旋
        A->left=RRRotation(A->left);
        return LLRotation(A);
    }
    AVLTree<T>* RLRotation(AVLTree<T>* A){
        //A的平衡被破坏,它的右子树的左子树(C)上有新插入的节点,要进行右左双旋
        /* A          A           C
             B   ->     C  ->    A   B
           C              B
         */
        //先变成右右旋转的格式,需要左旋B
        A->right=LLRotation(A->right);
        return RRRotation(A);

    }
    AVLTree<T>* Insert(AVLTree<T>* root,T x){
        //这里不能重起变量名必须用root命名,和参数名一样,为了始终return root
        //即不能 if (!root) AVLTree<T>* root=new AVLTree<T>(x);
        //★★★★★★上面的说明很重要!!!!!
        if (!root) root=new AVLTree<T>(x);
        else{
            if (x<root->data){
                root->left=Insert(root->left,x);
                //判断是否需要旋转,只需判断左子树减右子树,因为刚刚插入了左子树
                if (getHeight(root->left)-getHeight(root->right)>=2){
                    //需要旋转,判断哪种旋转
                    if (x<root->left->data) root=LLRotation(root);    //插到了左子树的左子树上,注意这里是root->left,不是root->left->left
                    else root=LRRotation(root);
                }
            }
            else{
                root->right=Insert(root->right,x); //和if对照
                if (getHeight(root->right)-getHeight(root->left)>=2){
                    if (x>root->right->data) root=RRRotation(root);
                    else root=RLRotation(root);
                }
            }
        }
        //别忘了更新树高☆
        root->height=reSetHeight(root);
        return root;
    }
};

int main(){
    //测试平衡二叉树
    int data;
    cin >> data;
    AVLTree<int>* root=new AVLTree<int>(data);
    //cout << root->data;
    while (data!=-1){
        cin >> data;
        root=root->Insert(root,data);
        cout << "root->data=" << root->data << endl;
    }
    return 0;
}
```

