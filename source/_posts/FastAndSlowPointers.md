---
title: FastAndSlowPointers
katex: true
date: 2024-12-30 21:11:13
excerpt: 快慢指针法框架
tag: ds
---
来自[27. 移除元素](https://programmercarl.com/0027.%E7%A7%BB%E9%99%A4%E5%85%83%E7%B4%A0.html)

**定义快慢指针**
- **快指针：寻找新数组的元素 ，新数组就是不含有目标元素的数组**
- **慢指针：指向更新 新数组下标的位置**

框架如下：
```cpp
// 时间复杂度：O(n)
// 空间复杂度：O(1)
class Solution {
public:
    int removeElement(vector<int>& nums, int val) {
        int slowIndex = 0;
        for (int fastIndex = 0; fastIndex < nums.size(); fastIndex++) {
            if (val != nums[fastIndex]) {
                nums[slowIndex++] = nums[fastIndex];
            }
        }
        return slowIndex;
    }
};
```

