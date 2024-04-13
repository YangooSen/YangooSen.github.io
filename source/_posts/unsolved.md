---
title: unsolved
katex: true
date: 2024-04-07 19:54:31
excerpt: 多次面对无法解决的leetcode题目
tag: ds
---
# 去重
- [第15题. 三数之和](https://programmercarl.com/0015.%E4%B8%89%E6%95%B0%E4%B9%8B%E5%92%8C.html#%E7%AE%97%E6%B3%95%E5%85%AC%E5%BC%80%E8%AF%BE)，想不到排序后双指针。问题的关键是三元组内的元素可用重复，但是三元组这个整体不能再次重复。用set<set<>>哈希法记录组合会有几个超时例。关注解法中对a的去重，结果集是i和i右边的某两个元素，组内可以重复，nums[i]!=nums[i-1]是为了避免上一次范围覆盖这一次，nums[i]是每次迭代保证三元组不会重复的关键，因为可用范围是一直在缩小的。
> 相关问题 [第18题. 四数之和](https://programmercarl.com/0018.%E5%9B%9B%E6%95%B0%E4%B9%8B%E5%92%8C.html#%E7%AE%97%E6%B3%95%E5%85%AC%E5%BC%80%E8%AF%BE)

# 原地操作
- [替换数字](https://programmercarl.com/kama54.%E6%9B%BF%E6%8D%A2%E6%95%B0%E5%AD%97.html#%E6%80%9D%E8%B7%AF)，原地操作经常用到双指针，数组填充类的问题，其做法都是先预先给数组扩容带填充后的大小，然后在从后向前进行操作,这样避免了重复向后移动已有元素的$O(n^2)$算法
- [翻转字符串里的单词](https://programmercarl.com/0151.%E7%BF%BB%E8%BD%AC%E5%AD%97%E7%AC%A6%E4%B8%B2%E9%87%8C%E7%9A%84%E5%8D%95%E8%AF%8D.html)和[右旋字符串](https://programmercarl.com/kama55.%E5%8F%B3%E6%97%8B%E5%AD%97%E7%AC%A6%E4%B8%B2.html)，想不到整体翻转再局部翻转的操作
-
# 奇思妙想
- [459.重复的子字符串](https://programmercarl.com/0459.%E9%87%8D%E5%A4%8D%E7%9A%84%E5%AD%90%E5%AD%97%E7%AC%A6%E4%B8%B2.html#%E7%AE%97%E6%B3%95%E5%85%AC%E5%BC%80%E8%AF%BE)真的想不到next数组还能在这用，直接看[简单推理](https://programmercarl.com/0459.%E9%87%8D%E5%A4%8D%E7%9A%84%E5%AD%90%E5%AD%97%E7%AC%A6%E4%B8%B2.html#%E6%80%9D%E8%B7%AF)配合想象画面好理解一点，但还是想不到。[暴力枚举](https://leetcode.cn/problems/repeated-substring-pattern/solutions/386481/zhong-fu-de-zi-zi-fu-chuan-by-leetcode-solution/)也需要思考
- [19.删除链表的倒数第N个节点](https://programmercarl.com/0019.%E5%88%A0%E9%99%A4%E9%93%BE%E8%A1%A8%E7%9A%84%E5%80%92%E6%95%B0%E7%AC%ACN%E4%B8%AA%E8%8A%82%E7%82%B9.html)只能想到用vector存储节点再删除，想不到双指针保持距离n删除
# 算法
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

