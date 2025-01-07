---
title: unsolved
katex: true
date: 2024-04-07 19:54:31
excerpt: 多次面对无法解决的leetcode题目
tag: ds
---
# 很绕
- [404.左叶子之和](https://programmercarl.com/0404.%E5%B7%A6%E5%8F%B6%E5%AD%90%E4%B9%8B%E5%92%8C.html#%E7%AE%97%E6%B3%95%E5%85%AC%E5%BC%80%E8%AF%BE),注意左叶子的判定：是叶子节点，并且是某个节点的左节点
```cpp

class Solution {
public:
    int sum=0;
    bool isLeaves(TreeNode* root){
        if (!root->left && !root->right) return true;
        else return false;
    }
    void sumLeaves(TreeNode* root){
        if (root->left){
            if (isLeaves(root->left)) sum+=root->left->val;
            sumLeaves(root->left);
        }
        if (root->right) sumLeaves(root->right);

    }

    int sumOfLeftLeaves(TreeNode* root) {
        //怎么定义左叶子？是叶子结点，而且是某个节点的左节点
        sumLeaves(root);
        return sum;
    }
};



```





# 去重
- [第15题. 三数之和](https://programmercarl.com/0015.%E4%B8%89%E6%95%B0%E4%B9%8B%E5%92%8C.html#%E7%AE%97%E6%B3%95%E5%85%AC%E5%BC%80%E8%AF%BE)，想不到排序后双指针。问题的关键是三元组内的元素可用重复，但是三元组这个整体不能再次重复。用set<set<>>哈希法记录组合会有几个超时例。关注解法中对a的去重，结果集是i和i右边的某两个元素，组内可以重复，nums[i]!=nums[i-1]是为了避免上一次范围覆盖这一次，nums[i]是每次迭代保证三元组不会重复的关键，因为可用范围是一直在缩小的。
```cpp
/*
 * @lc app=leetcode.cn id=15 lang=cpp
 *
 * [15] 三数之和
 */

// @lc code=start
class Solution {
public:
    vector<vector<int>> threeSum(vector<int>& nums) {
        sort(nums.begin(),nums.end());
        vector<vector<int>> res;
        for (int i=0;i<nums.size();i++){
            if (i>0 && nums[i]==nums[i-1]) continue;
            int left=i+1,right=nums.size()-1;
            while (left<right){
                if (nums[left]+nums[right]+nums[i]>0) right--;
                else if (nums[left]+nums[right]+nums[i]<0) left++;
                else{
                    res.push_back(vector<int>{nums[i],nums[left],nums[right]});
                    left++;right--;
                    while (left<nums.size() && nums[left]==nums[left-1]) left++;
                    while (right>0 && nums[right]==nums[right+1]) right--;
                }
            }

        }
        return res;
    }
};
// @lc code=end



```

> 相关问题 [第18题. 四数之和](https://programmercarl.com/0018.%E5%9B%9B%E6%95%B0%E4%B9%8B%E5%92%8C.html#%E7%AE%97%E6%B3%95%E5%85%AC%E5%BC%80%E8%AF%BE)，注意给的数不超过`int`,相加之后可能会超过`int`,但是`target`是`int`,所有只要比较的时候加`long`强转就可以了,后续不需要加。哪里溢出就先只在那个地方上调，而不要从源头就自上而下都上调

> 注意双指针的问题一般都是用`while(left<right)`解决，而不要用`for`,`for`循环不好掌控

- [40.组合总和II](https://programmercarl.com/0040.%E7%BB%84%E5%90%88%E6%80%BB%E5%92%8CII.html#%E6%80%9D%E8%B7%AF)，完全想不到用used数组，used[i]=true表示同一树枝上v[i]用过，很好理解，因为往下层递归的时候会先标记用过。**used[i]=false表示同一树层上v[i]用过**，比较难想到。可以看图想，每个树层每次是从[i,)区间选的，前一个数用过之后used就会复原成false。因此排序之后v[i-1]==v[i]表示和前一个数字一样，且used[i-1]==false复原了，说明同一树层用过了，那么就跳过。如果少used[i-1]==false条件，就会在树枝层面也进行去重，[1,1,6]就会被去掉。其实for循环中 [start,i）这个区间的值，同一树层已经用过了，因此也可以用i > startIndex判断。startIndex是用来限制备选candidates数组的，i>startIndex说明同一树层已经遍历到i了，前面的已经用过了。关注startIndex和i的实际代表意义,同一树层遍历是 [start,i]



# 原地操作
- [替换数字](https://programmercarl.com/kama54.%E6%9B%BF%E6%8D%A2%E6%95%B0%E5%AD%97.html#%E6%80%9D%E8%B7%AF)，原地操作经常用到双指针，数组填充类的问题，其做法都是先预先给数组扩容带填充后的大小，然后在从后向前进行操作,这样避免了重复向后移动已有元素的$O(n^2)$算法
- [翻转字符串里的单词](https://programmercarl.com/0151.%E7%BF%BB%E8%BD%AC%E5%AD%97%E7%AC%A6%E4%B8%B2%E9%87%8C%E7%9A%84%E5%8D%95%E8%AF%8D.html)和[右旋字符串](https://programmercarl.com/kama55.%E5%8F%B3%E6%97%8B%E5%AD%97%E7%AC%A6%E4%B8%B2.html)，想不到整体翻转再局部翻转的操作
-
# 奇思妙想
- [459.重复的子字符串](https://programmercarl.com/0459.%E9%87%8D%E5%A4%8D%E7%9A%84%E5%AD%90%E5%AD%97%E7%AC%A6%E4%B8%B2.html#%E7%AE%97%E6%B3%95%E5%85%AC%E5%BC%80%E8%AF%BE)真的想不到next数组还能在这用，直接看[简单推理](https://programmercarl.com/0459.%E9%87%8D%E5%A4%8D%E7%9A%84%E5%AD%90%E5%AD%97%E7%AC%A6%E4%B8%B2.html#%E6%80%9D%E8%B7%AF)配合想象画面好理解一点，但还是想不到。[暴力枚举](https://leetcode.cn/problems/repeated-substring-pattern/solutions/386481/zhong-fu-de-zi-zi-fu-chuan-by-leetcode-solution/)也需要思考
- [19.删除链表的倒数第N个节点](https://programmercarl.com/0019.%E5%88%A0%E9%99%A4%E9%93%BE%E8%A1%A8%E7%9A%84%E5%80%92%E6%95%B0%E7%AC%ACN%E4%B8%AA%E8%8A%82%E7%82%B9.html)只能想到用vector存储节点再删除，想不到双指针保持距离n删除
# 算法
- [98.验证二叉搜索树](https://programmercarl.com/0098.%E9%AA%8C%E8%AF%81%E4%BA%8C%E5%8F%89%E6%90%9C%E7%B4%A2%E6%A0%91.html)自己想到的是表达二叉树的区间来做（见下图），没想到直接用中序遍历
```cpp
class Solution {
public:
    bool isValid(TreeNode* root,long minRange,long maxRange){
        if (!root) return true;
        if (root->left){
            if (root->left->val>=root->val) return false;
            if (root->left->val<=minRange) return false;
        }

        if (root->right){
            if (root->right->val<=root->val) return false;
            if (root->right->val>=maxRange) return false;
        }
        long tmp;

        //右子树的最小值都至少是root->val或者更大
        tmp=minRange>root->val?minRange:root->val;
        bool right=isValid(root->right,tmp,maxRange);
        
        //左子树的最大值都至多是root->val或者更小
        tmp=maxRange<root->val?maxRange:root->val;
        bool left=isValid(root->left,minRange,tmp);

        return left && right;
    }
    bool isValidBST(TreeNode* root) {
        return isValid(root,-2147483649,2147483648);
    }
};

```
![例子](illustration.png)
- [106.从中序与后序遍历序列构造二叉树](https://programmercarl.com/0106.%E4%BB%8E%E4%B8%AD%E5%BA%8F%E4%B8%8E%E5%90%8E%E5%BA%8F%E9%81%8D%E5%8E%86%E5%BA%8F%E5%88%97%E6%9E%84%E9%80%A0%E4%BA%8C%E5%8F%89%E6%A0%91.html)每次都是有思路但报错，最重要的是注意划分后序遍历不要去找右子树的节点，而是直接用[len判断位置](https://blog.csdn.net/weixin_52812620/article/details/126631784)啊
```cpp
tree* create(int postStart,int postEnd,int inStart,int inEnd){
    if (postStart>postEnd || inStart>inEnd) return nullptr;
    tree* root=new tree(post[postEnd]);
    //cout << root->data << endl;
    int i=inStart;
    while (in[i]!=root->data){i++;}
    int len=i-inStart; //这里用len判断是关键，不然总报错
    //cout << len;
    //cout << in[i+1] << ' ' << in[inEnd] << endl;
    //cout << post[postStart+len] << ' ' << post[postEnd-1] << endl;
    root->left=create(postStart,postStart+len-1,inStart,i-1);
    root->right=create(postStart+len,postEnd-1,i+1,inEnd);
    return root;
}

```
- [101. 对称二叉树](https://programmercarl.com/0101.%E5%AF%B9%E7%A7%B0%E4%BA%8C%E5%8F%89%E6%A0%91.html#%E7%AE%97%E6%B3%95%E5%85%AC%E5%BC%80%E8%AF%BE)自己写的层序遍历判断，想的时候考虑过递归但没想到具体思路。一个node参数的函数解决不了就自己写一个两个参数的递归函数
- [225. 用队列实现栈](https://programmercarl.com/0225.%E7%94%A8%E9%98%9F%E5%88%97%E5%AE%9E%E7%8E%B0%E6%A0%88.html#%E7%AE%97%E6%B3%95%E5%85%AC%E5%BC%80%E8%AF%BE)第三次做的时候想了一段时间想到了，还是不熟练，队列模拟栈用一个`queue`和`queue_copy`解决
```cpp
/*
 * @lc app=leetcode.cn id=225 lang=cpp
 *
 * [225] 用队列实现栈
 */

// @lc code=start
class MyStack {
public:
    queue<int> q;
    queue<int> q_copy;
    MyStack() {

    }
    
    void push(int x) {
        q.push(x);
    }
    
    int pop() {
        int x;
        while (q.size()>1){
            x=q.front();q.pop();
            q_copy.push(x);
        }
        x=q.front();q.pop();

        while (!q_copy.empty()){
            int i=q_copy.front();q_copy.pop();
            q.push(i);
        }

        return x;
    }
    
    int top() {
        int x;
        while (q.size()>1){
            x=q.front();q.pop();
            q_copy.push(x);
        }
        x=q.front();q.pop();
        q_copy.push(x);

        while (!q_copy.empty()){
            int i=q_copy.front();q_copy.pop();
            q.push(i);
        }

        return x;

    }
    
    bool empty() {
        return q.empty() && q_copy.empty();
    }
};

/**
 * Your MyStack object will be instantiated and called as such:
 * MyStack* obj = new MyStack();
 * obj->push(x);
 * int param_2 = obj->pop();
 * int param_3 = obj->top();
 * bool param_4 = obj->empty();
 */
// @lc code=end



```
> 相关问题 [232.用栈实现队列](https://programmercarl.com/0232.%E7%94%A8%E6%A0%88%E5%AE%9E%E7%8E%B0%E9%98%9F%E5%88%97.html)和队列模拟栈的思路不同，需要来回倒腾
- [KMP](https://programmercarl.com/0028.%E5%AE%9E%E7%8E%B0strStr.html#%E7%AE%97%E6%B3%95%E5%85%AC%E5%BC%80%E8%AF%BE)总是学了忘
```cpp
void getNext(vector<int>& next, const string& s) {
    int j = 0;
    next[0] = 0;
    for(int i = 1; i < s.size(); i++) {
        while (j > 0 && s[i] != s[j]) j = next[j - 1]; //s[0,j]的前缀字符已经不匹配了,看看s[0,j-1]的前缀字符还是否能匹配
        if (s[i] == s[j]) j++;
        next[i] = j;
    }
}
```
> next[i]是s[0,i]的字符串中，最长的相同前缀和后缀的长度。因此i是表示的是后缀末尾（这样才能用next[i]来表示s[0,i]这段字符串），j是前缀末尾（这样才能直接用j表示长度）。动态规划得到next[i]，如果不相等就用之前得到的最长相等长度
```cpp
int KMP(string main, string pattern) {

    if (pattern.size() == 0) return 0;
    vector<int> next(pattern.size(),0);
    getNext(next, pattern);

    int j = 0;
    for (int i = 0; i < main.size(); i++) {
        while(j > 0 && main[i] != pattern[j]) j=next[j-1]; //当前字符不相同，退回到pattern前缀和"main后缀"匹配的地方
        if (main[i] == pattern[j]) j++; //当前字符相同，继续匹配
        if (j == pattern.size()) return (i - pattern.size() + 1); //匹配成功
    }


    return -1;
}

```
退回到pattern前缀和"main后缀"（其实是pattern后缀，因为已经j-1前的已经匹配了，所以pattern后缀就是"main后缀"，理解了这个就能理解getNext的循环和主函数的循环几乎一致的代码）匹配的地方，看下面的例子理解


![例子](example.gif)


- [239. 滑动窗口最大值](https://programmercarl.com/0239.%E6%BB%91%E5%8A%A8%E7%AA%97%E5%8F%A3%E6%9C%80%E5%A4%A7%E5%80%BC.html#%E7%AE%97%E6%B3%95%E5%85%AC%E5%BC%80%E8%AF%BE)单调队列解决，没见过的结构，[实现](https://zhuanlan.zhihu.com/p/346354943)的关键是只维护(push,pop)那些有可能成为最值的数。还是从前面出，从后面进，只和队口元素比较，出的时候和出口的最值比较，判断是否要pop，入的时候和入口的最值比较，判断是否push，**始终保持单调性**

- [236. 二叉树的最近公共祖先](https://programmercarl.com/0236.%E4%BA%8C%E5%8F%89%E6%A0%91%E7%9A%84%E6%9C%80%E8%BF%91%E5%85%AC%E5%85%B1%E7%A5%96%E5%85%88.html#%E7%AE%97%E6%B3%95%E5%85%AC%E5%BC%80%E8%AF%BE)，自己做成的是把路径收集起来找分叉的那个点，没想到这里的后序遍历怎么做，[相似问题:235. 二叉搜索树的最近公共祖先](https://programmercarl.com/0235.%E4%BA%8C%E5%8F%89%E6%90%9C%E7%B4%A2%E6%A0%91%E7%9A%84%E6%9C%80%E8hh%BF%91%E5%85%AC%E5%85%B1%E7%A5%96%E5%85%88.html)但需要思考，还是能用自己的方法做

- [注意节点删除和AVL的节点插入](https://blog.csdn.net/weixin_52812620/article/details/126631784)，总是不会写记不住，可以和[108.将有序数组转换为二叉搜索树](https://programmercarl.com/0108.%E5%B0%86%E6%9C%89%E5%BA%8F%E6%95%B0%E7%BB%84%E8%BD%AC%E6%8D%A2%E4%B8%BA%E4%BA%8C%E5%8F%89%E6%90%9C%E7%B4%A2%E6%A0%91.html#%E7%AE%97%E6%B3%95%E5%85%AC%E5%BC%80%E8%AF%BE)做对比，这个题见到还以为要写节点插入，马上放弃了，结果换一种思路很简单（lc判定为easy）

```cpp
class Solution {
public:
    TreeNode* findMin(TreeNode* root){
        TreeNode* cur=root;
        while (cur->left){
            cur=cur->left;
        }
        return cur;
    }
    TreeNode* deleteNode(TreeNode* root, int key) {
        
        if (!root) return root;
        if (key<root->val) root->left=deleteNode(root->left,key);
        else if (key>root->val) root->right=deleteNode(root->right,key);
        else{ //相等
            if (!root->left && !root->right){
                delete root;
                return nullptr;
            }
            else if (!root->left && root->right){
                TreeNode* res=root->right;
                delete root;
                return res;
            }
            else if (root->left && !root->right){
                TreeNode* res=root->left;
                delete root;
                return res;
            }
            else{
                //有两个儿子,以右子树的最小值代替此节点，然后删除右子树的最小值
                TreeNode* minNode=findMin(root->right);
                root->val=minNode->val;
                root->right=deleteNode(root->right,minNode->val);
                return root;
            }

        }
        return root; 

    }
};

```
- [669. 修剪二叉搜索树](https://programmercarl.com/0669.%E4%BF%AE%E5%89%AA%E4%BA%8C%E5%8F%89%E6%90%9C%E7%B4%A2%E6%A0%91.html#%E7%AE%97%E6%B3%95%E5%85%AC%E5%BC%80%E8%AF%BE)是自己做出来的，就是先写好一个有点问题但能跑通几个案例的代码，然后根据没跑通的再去修改，注意这种`迭代的工程思维`。最好还是看看怎么做，防止以后写不出


- [538.把二叉搜索树转换为累加树](https://programmercarl.com/0538.%E6%8A%8A%E4%BA%8C%E5%8F%89%E6%90%9C%E7%B4%A2%E6%A0%91%E8%BD%AC%E6%8D%A2%E4%B8%BA%E7%B4%AF%E5%8A%A0%E6%A0%91.html#%E7%AE%97%E6%B3%95%E5%85%AC%E5%BC%80%E8%AF%BE)，有想过中序遍历是递增，但发现前序后后序遍历都不是递减，就自己写了中序存数组再反着遍历的代码。没想到`反中序遍历右中左`就是递减情况
# 数学

- [142.环形链表II](https://programmercarl.com/0142.%E7%8E%AF%E5%BD%A2%E9%93%BE%E8%A1%A8II.html#%E7%AE%97%E6%B3%95%E5%85%AC%E5%BC%80%E8%AF%BE)，想不到双指针的思路，也想不到最后的`x=z`
> 相关问题 [160.链表相交](https://programmercarl.com/%E9%9D%A2%E8%AF%95%E9%A2%9802.07.%E9%93%BE%E8%A1%A8%E7%9B%B8%E4%BA%A4.html#%E6%80%9D%E8%B7%AF) 每次都是用map解决的，双指针对齐之后也可以解决


# 模拟
- [中缀表达式求值](https://blog.csdn.net/weixin_43941332/article/details/105084051?utm_medium=distribute.pc_relevant.none-task-blog-2~default~baidujs_baidulandingword~default-0-105084051-blog-104799083.235^v43^pc_blog_bottom_relevance_base2&spm=1001.2101.3001.4242.1&utm_relevant_index=3),后缀表达式一个栈存数字，直接遇到符号处理即可，**前缀表达式可以倒着遍历就是后缀了**。前缀表达式需要两个栈，有[动画视频](https://www.bilibili.com/video/BV1H4411N7oD?p=21)![操作](op.png)
> 注意左括号在栈外面是优先级最高的，需要压入栈，而栈内的左括号是优先级最低的，比所有符号都低，就算外面是+-，也不能直接运算

# 其他

-[39. 组合总和](https://programmercarl.com/0039.%E7%BB%84%E5%90%88%E6%80%BB%E5%92%8C.html#%E7%AE%97%E6%B3%95%E5%85%AC%E5%BC%80%E8%AF%BE)，自己写的时候担心for循环里没有正常start+1，回溯会推进不下去。其实根据target可以推进下去，自己想当然以为做法不对。

- [131.分割回文串](https://programmercarl.com/0131.%E5%88%86%E5%89%B2%E5%9B%9E%E6%96%87%E4%B8%B2.html#%E7%AE%97%E6%B3%95%E5%85%AC%E5%BC%80%E8%AF%BE)可能想不到用回溯法解决。边界问题很关键，一开始直接在bt的第一个if判断里return了，导致bb这样的字符串不AC。切割字符串的函数是substr(0,i) 和 substr(i,size-i)。没有i+1。很相似的一道分割字符串题目可以一起做[93.复原IP地址](https://programmercarl.com/0093.%E5%A4%8D%E5%8E%9FIP%E5%9C%B0%E5%9D%80.html#%E7%AE%97%E6%B3%95%E5%85%AC%E5%BC%80%E8%AF%BE)
```cpp
/*
 * @lc app=leetcode.cn id=131 lang=cpp
 *
 * [131] 分割回文串
 */

// @lc code=start
class Solution {
public:
    bool isHuiwen(const string& s){
        if (s.size()==0 || s.size()==1) return true;
        int left=0,right=s.size()-1;
        while (left<=right){
            if (s[left]!=s[right]) return false;
            left++;right--;
        }
        return true;
    }

    vector<string> path;
    vector<vector<string>> res;

    void bt(const string& s){
        if (isHuiwen(s)){
            path.push_back(s);
            res.push_back(path);
            path.pop_back();
        }
        
        string leftString,rightString;
        for (int i=1,size=s.size();i<size;i++){
            //分割字符串
            leftString=s.substr(0,i);
            if (!isHuiwen(leftString)) continue;
            if (leftString!="") path.push_back(leftString);
            rightString=s.substr(i,size-i);
            // cout << leftString << " " << rightString << endl;
            bt(rightString);
            if (leftString!="") path.pop_back();
            
        }
    }

    vector<vector<string>> partition(string s) {
        bt(s);
        // cout << s.substr(0,3);
        return res;
    }
};
```
