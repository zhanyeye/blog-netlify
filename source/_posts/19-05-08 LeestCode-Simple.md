---
title: LeestCode-Simple
date: 2019-05-08 21:21:58
tags:
categories:
- LeetCode
---



###### 20. Valid Parentheses

Easy

Given a string containing just the characters `'('`, `')'`, `'{'`, `'}'`, `'['` and `']'`, determine if the input string is valid.

An input string is valid if:

1. Open brackets must be closed by the same type of brackets.
2. Open brackets must be closed in the correct order.

Note that an empty string is also considered valid.

<!--more-->

**Example 1:**

```
Input: "()"
Output: true
```

**Example 2:**

```
Input: "()[]{}"
Output: true
```

**Example 3:**

```
Input: "(]"
Output: false
```

**Example 4:**

```
Input: "([)]"
Output: false
```

**Example 5:**

```
Input: "{[]}"
Output: true
```



```c++
class Solution {
public:
    bool isValid(string s) {
        map<char, char> mp;
        stack<char> st;
        mp['('] = ')';
        mp['{'] = '}';
        mp['['] = ']';
        for (int i = 0; i < s.size(); i++) {
            if (s[i] == '(' || s[i] == '{' || s[i] == '[') {
                st.push(s[i]);
            } else {
                if (!st.empty() && mp[st.top()] == s[i]) {
                    st.pop();
                } else {
                    return false;
                }
            }
        }
        return st.empty() ? true : false;
        
    }
};
```



###### 26. Remove Duplicates from Sorted Array

Easy

Given a sorted array *nums*, remove the duplicates [**in-place**](https://en.wikipedia.org/wiki/In-place_algorithm) such that each element appear only *once* and return the new length.

Do not allocate extra space for another array, you must do this by **modifying the input array in-place** with O(1) extra memory.

**Example 1:**

```
Given nums = [1,1,2],

Your function should return length = 2, with the first two elements of nums being 1 and 2 respectively.

It doesn't matter what you leave beyond the returned length.
```

**Example 2:**

```
Given nums = [0,0,1,1,1,2,2,3,3,4],

Your function should return length = 5, with the first five elements of nums being modified to 0, 1, 2, 3, and 4 respectively.

It doesn't matter what values are set beyond the returned length.
```

**Clarification:**

Confused why the returned value is an integer but your answer is an array?

Note that the input array is passed in by **reference**, which means modification to the input array will be known to the caller as well.

Internally you can think of this:

```
// nums is passed in by reference. (i.e., without making a copy)
int len = removeDuplicates(nums);

// any modification to nums in your function would be known by the caller.
// using the length returned by your function, it prints the first len elements.
for (int i = 0; i < len; i++) {
    print(nums[i]);
}
```

[题解](https://github.com/azl397985856/leetcode/blob/master/problems/26.remove-duplicates-from-sorted-array.md)

**思路**

使用快慢指针来记录遍历的坐标。

- 开始时这两个指针都指向第一个数字
- 如果两个指针指的数字相同，则快指针向前走一步
- 如果不同，则两个指针都向前走一步
- 当快指针走完整个数组后，慢指针当前的坐标加1就是数组中不同数字的个数



**关键点解析**

- 双指针

  这道题如果不要求，O(n)的时间复杂度， O(1)的空间复杂度的话，会很简单。 但是这道题是要求的，这种题的思路一般都是采用双指针

- 如果是数据是无序的，就不可以用这种方式了，从这里也可以看出排序在算法中的基础性和重要性。

```c++
class Solution {
public:
    // 使用快慢指针来记录遍历坐标
    // 初始时slowp = 0, fastp = 0
    // 若 nums[slowp] == nums[fastp]  fastp++
    // 若 nums[slowp] != nums[fastp]  slowp++; nums[slowp] = nums[fastp]; fastp++
    // 解释：
    // 在 nums[slowp] == nums[fastp], slowp 和 fastp 之间的元素都是相同的
    // 当 nums[slowp] != nums[fastp], fastp 找到了一个不相同的元素，slowp++,用nums[fastp] 去覆盖 nums[slowp]
    int removeDuplicates(vector<int>& nums) {
        if (nums.size() == 0) {
            return 0;
        }
        int slowp = 0;
        for (int fastp = 0; fastp < nums.size(); fastp++) {
            if (nums[slowp] != nums[fastp]) {
                slowp++;
                nums[slowp] = nums[fastp];
            }
        }
        return slowp + 1;
    }
};
```



###### 88. Merge Sorted Array

Easy

Given two sorted integer arrays *nums1* and *nums2*, merge *nums2* into *nums1* as one sorted array.

**Note:**

- The number of elements initialized in *nums1* and *nums2* are *m* and *n* respectively.
- You may assume that *nums1* has enough space (size that is greater or equal to *m* + *n*) to hold additional elements from *nums2*.

**Example:**

```
Input:
nums1 = [1,2,3,0,0,0], m = 3
nums2 = [2,5,6],       n = 3

Output: [1,2,2,3,5,6]
```


  因为是原地修改所以只能从后向前：

+ cur 表示当前要填充的位置，初始化为 m + n - 1
+ pm 指向 nums1  的最后一个元素，pn 指向num2 的最后一个元素
+ while cur >= 0 
  + if pm < 0 ：将nums2 中剩余的元素插入到 nums1
  + if pn < 0 ： nums2 中元素已经全部插完，return 
  + else 选择 nums1[pm] 和 nums2[pn] 中小的插入 nums[cur]

```c++
class Solution {
public:
    void merge(vector<int>& nums1, int m, vector<int>& nums2, int n) {
        int cur = m + n -1;
        int pm = m - 1, pn = n - 1;
        while (cur >= 0) {
            if (pm < 0) {
                nums1[cur--] = nums2[pn--];
                continue;
            }
            if (pn < 0) {
                return;
            }
            if (nums1[pm] > nums2[pn]) {
                nums1[cur--] = nums1[pm--];    
            } else {
                nums1[cur--] = nums2[pn--];
            }
        }
    }
};
```

