---
title: BackTracking
katex: true
date: 2024-12-30 20:47:52
excerpt: 回溯法框架
tag: ds
---

回溯是暴力搜索算法，并不高效，可以解决
- 组合问题：N个数里面按一定规则找出k个数的集合
- 排列问题：N个数按一定规则全排列，有几种排列方式
- 切割问题：一个字符串按一定规则有几种切割方式
- 子集问题：一个N个数的集合里有多少符合条件的子集
- 棋盘问题：N皇后，解数独等等

回溯形成的是一个树形结构，for循环横向遍历，递归纵向遍历

**剪枝精髓**：for循环在寻找起点的时候要有一个范围，如果这个起点到集合终止之间的元素已经不可能达到题目要求的规则了，就没有必要搜索了。


来自[回溯法模板](https://programmercarl.com/%E5%9B%9E%E6%BA%AF%E7%AE%97%E6%B3%95%E7%90%86%E8%AE%BA%E5%9F%BA%E7%A1%80.html#%E5%9B%9E%E6%BA%AF%E6%B3%95%E6%A8%A1%E6%9D%BF)
```cpp
void backtracking(参数) {
    if (终止条件) {
        存放结果;
        return;
    }

    for (选择：本层集合中元素（树中节点孩子的数量就是集合的大小）) {
        处理节点;
        backtracking(路径，选择列表); // 递归
        回溯，撤销处理结果
    }
}
```
![回溯法遍历情况](1.png)
如果需要去重，可以参考[90.子集II](https://programmercarl.com/0090.%E5%AD%90%E9%9B%86II.html#%E6%80%9D%E8%B7%AF)，[40.组合总和II](https://programmercarl.com/0040.%E7%BB%84%E5%90%88%E6%80%BB%E5%92%8CII.html#%E5%9B%9E%E6%BA%AF%E4%B8%89%E9%83%A8%E6%9B%B2)，[47.全排列 II](https://programmercarl.com/0047.%E5%85%A8%E6%8E%92%E5%88%97II.html#_47-%E5%85%A8%E6%8E%92%E5%88%97-ii)，它们都是 **有重复元素但不能有重复组合** 。 used 数组表示的是同一树枝上是否重复使用，used[i]==1 表示同一树枝上已使用；used[i]==0 表示同一树枝上没使用，如果此时给定数组出现 nums[i-1]==nums[i] 就需要 continue，因为同一树层不能重复选，在
![这种情况](2.png)

如果不能排序去重，参考[491.递增子序列](https://programmercarl.com/0491.%E9%80%92%E5%A2%9E%E5%AD%90%E5%BA%8F%E5%88%97.html) 使用 unordered_set，处理节点前，先 if uset.find(nums[i]) != uset.end()，处理节点之后标记使用 uset.insert(nums[i])
