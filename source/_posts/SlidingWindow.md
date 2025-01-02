---
title: SlidingWindow
katex: true
date: 2024-12-30 20:55:50
excerpt: 滑动窗口框架
tag: ds
---

来自[我写了首诗，把滑动窗口算法变成了默写题](https://labuladong.github.io/algo/di-ling-zh-bfe1b/wo-xie-le--f02cd/)
- 要注意下面的滑动窗口框架表示的范围是[left,right)，左闭右开区间
- 一般用来解决连续子数组问题
```cpp
/* 滑动窗口算法框架 */
void slidingWindow(string s) {
    unordered_map<char, int> window;

    int left = 0, right = 0;
    while (right < s.size()) {
        // c 是将移入窗口的字符
        char c = s[right];
        // 增大窗口
        right++;
        // 进行窗口内数据的一系列更新
        ...

        /*** debug 输出的位置 ***/
        printf("window: [%d, %d)\n", left, right);
        /********************/

        // 判断左侧窗口是否要收缩
        //需要收缩的条件一般是:
        // 1)不满足要求:比如要求的数字和超过了target
        // 2)窗口更小也满足要求:需要的字符溢出了需求,缩小窗口减少溢出
        while (window needs shrink) {
            // d 是将移出窗口的字符
            char d = s[left];
            // 缩小窗口
            left++;
            // 进行窗口内数据的一系列更新
            ...
        }
        //观察是否更新最终结果
    }
}
```

