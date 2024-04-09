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

# 原地工作
- [替换数字](https://programmercarl.com/kama54.%E6%9B%BF%E6%8D%A2%E6%95%B0%E5%AD%97.html#%E6%80%9D%E8%B7%AF)，原地操作经常用到双指针，数组填充类的问题，其做法都是先预先给数组扩容带填充后的大小，然后在从后向前进行操作,这样避免了重复向后移动已有元素的$O(n^2)$算法
