---
title: unsolved
katex: true
date: 2024-04-07 19:54:31
excerpt: 多次面对无法解决的leetcode题目
tag: ds
---
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

# 原地操作
- [替换数字](https://programmercarl.com/kama54.%E6%9B%BF%E6%8D%A2%E6%95%B0%E5%AD%97.html#%E6%80%9D%E8%B7%AF)，原地操作经常用到双指针，数组填充类的问题，其做法都是先预先给数组扩容带填充后的大小，然后在从后向前进行操作,这样避免了重复向后移动已有元素的$O(n^2)$算法
- [翻转字符串里的单词](https://programmercarl.com/0151.%E7%BF%BB%E8%BD%AC%E5%AD%97%E7%AC%A6%E4%B8%B2%E9%87%8C%E7%9A%84%E5%8D%95%E8%AF%8D.html)和[右旋字符串](https://programmercarl.com/kama55.%E5%8F%B3%E6%97%8B%E5%AD%97%E7%AC%A6%E4%B8%B2.html)，想不到整体翻转再局部翻转的操作
-
# 奇思妙想
- [459.重复的子字符串](https://programmercarl.com/0459.%E9%87%8D%E5%A4%8D%E7%9A%84%E5%AD%90%E5%AD%97%E7%AC%A6%E4%B8%B2.html#%E7%AE%97%E6%B3%95%E5%85%AC%E5%BC%80%E8%AF%BE)真的想不到next数组还能在这用，直接看[简单推理](https://programmercarl.com/0459.%E9%87%8D%E5%A4%8D%E7%9A%84%E5%AD%90%E5%AD%97%E7%AC%A6%E4%B8%B2.html#%E6%80%9D%E8%B7%AF)配合想象画面好理解一点，但还是想不到。[暴力枚举](https://leetcode.cn/problems/repeated-substring-pattern/solutions/386481/zhong-fu-de-zi-zi-fu-chuan-by-leetcode-solution/)也需要思考
- [19.删除链表的倒数第N个节点](https://programmercarl.com/0019.%E5%88%A0%E9%99%A4%E9%93%BE%E8%A1%A8%E7%9A%84%E5%80%92%E6%95%B0%E7%AC%ACN%E4%B8%AA%E8%8A%82%E7%82%B9.html)只能想到用vector存储节点再删除，想不到双指针保持距离n删除
# 算法
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
        while (j > 0 && s[i] != s[j]) j = next[j - 1];
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

# 数学
- [142.环形链表II](https://programmercarl.com/0142.%E7%8E%AF%E5%BD%A2%E9%93%BE%E8%A1%A8II.html#%E7%AE%97%E6%B3%95%E5%85%AC%E5%BC%80%E8%AF%BE)，想不到双指针的思路，也想不到最后的`x=z`
> 相关问题 [160.链表相交](https://programmercarl.com/%E9%9D%A2%E8%AF%95%E9%A2%9802.07.%E9%93%BE%E8%A1%A8%E7%9B%B8%E4%BA%A4.html#%E6%80%9D%E8%B7%AF) 每次都是用map解决的，双指针对齐之后也可以解决
