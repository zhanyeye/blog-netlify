---
title: leetcode 题解
date: 2019-04-30 17:32:39
tags:
categories:
- LeetCode
---



###### Two Sum

Easy  2019-04-30

Given an array of integers, return **indices** of the two numbers such that they add up to a specific target.

You may assume that each input would have **exactly** one solution, and you may not use the *same* element twice.

**Example:**

```
Given nums = [2, 7, 11, 15], target = 9,

Because nums[0] + nums[1] = 2 + 7 = 9,
return [0, 1].
```

+ 注意每个元素只能使用一次
+ 两个坑：
  + 当一个数和它的匹配相同是如 [3, 4]  target: 6;   6 - 3 = 3,但是这个3 不能再用了
  + [3, 3]     target: 6;   第一个3不能用，但是第二个3 还可以用



先将数和索引作为键值对加入map，再去从map中找target - nums[i]

```c++
//2019-04-30
class Solution {
public:
    vector<int> twoSum(vector<int>& nums, int target) {
        map<int, int> mp;
        vector<int> ans;
        for (int i = 0; i < nums.size(); i++) {
            mp[nums[i]] = i;
        }
        for (int i = 0; i < nums.size(); i++) {
            int need = target - nums[i];
            if (mp.count(need) && mp[need] != i) { //注意顺序不能反过来！因为mp[need]若不存在，会被初始化为0
                ans.push_back(i);
                ans.push_back(mp[need]);
                break;
            }
        }
        return ans;
    }
};
```

<!--more-->

The basic idea is to maintain a hash table for each element `num` in `nums`, using `num` as key and its index (`0`-based) as value. For each `num`, search for `target - num` in the hash table. If it is found and is not the same element as `num`, then we are done.

The code is as follows. Note that each time before we add `num` to `mp`, we search for `target - num` first and so we will not hit the same element.

```c++
class Solution {
public:
    vector<int> twoSum(vector<int>& nums, int target) {
        map<int, int> mp;
        for (int i = 0; i < nums.size(); i++) {
            int matching = target - nums[i];
            if (mp.find(matching) != mp.end()) {
                return {mp[matching], i};
            }
            mp[nums[i]] = i;
        }
        return {};
    }
};
```



###### Add Two Numbers

Medium  2019-05-01

You are given two **non-empty** linked lists representing two non-negative integers. The digits are stored in **reverse order** and each of their nodes contain a single digit. Add the two numbers and return it as a linked list.

You may assume the two numbers do not contain any leading zero, except the number 0 itself.

**Example:**

```
Input: (2 -> 4 -> 3) + (5 -> 6 -> 4)
Output: 7 -> 0 -> 8
Explanation: 342 + 465 = 807.
```



```c++
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
class Solution {
public:
    ListNode* addTwoNumbers(ListNode* l1, ListNode* l2) {
        ListNode dummyHead = ListNode(0);
        ListNode *p = l1, *q = l2, *curr = &dummyHead;
        int carry = 0;
        while (p != NULL || q != NULL) {
            int x = (p != NULL) ? p->val : 0;
            int y = (q != NULL) ? q->val : 0;
            int sum = x + y + carry;
            carry = sum / 10;
            curr->next = new ListNode(sum % 10);
            curr = curr->next;
            if (p != NULL) p = p->next;
            if (q != NULL) q = q->next;   
        }
        if (carry > 0) { //若最后一位还有进位
            curr->next = new ListNode(1);
        }
        return dummyHead.next;
    }
};
```



###### Longest Substring Without Repeating Characters

Medium  2019-05-02

Given a string, find the length of the **longest substring** without repeating characters.

**Example 1:**

```
Input: "abcabcbb"
Output: 3 
Explanation: The answer is "abc", with the length of 3. 
```

**Example 2:**

```
Input: "bbbbb"
Output: 1
Explanation: The answer is "b", with the length of 1.
```

**Example 3:**

```
Input: "pwwkew"
Output: 3
Explanation: The answer is "wke", with the length of 3. 
             Note that the answer must be a substring, "pwke" is a subsequence and not a substring.
```



O(n^2^)

```c++
class Solution {
public:
    int lengthOfLongestSubstring(string s) {
        set<char> st;
        int ans = 0;
        for (int i = 0; i < s.size(); i++) {
            int len = 0;
            int p = i;
            while (p < s.size()) {
                if (st.count(s[p])) {
                    st.clear();
                    break;
                } else {
                    st.insert(s[p]);
                    len++;
                    p++;
                }
            }
            ans = max(ans, len);
        }        
        return ans;
    }
};
```



Sliding Window

> To check if a character is already in the substring, we can scan the substring, which leads to an O(n^2) algorithm. But we can do better.
>
> By using HashSet as a sliding window, checking if a character in the current can be done in *O*(1).
>
> A sliding window is an abstract concept commonly used in array/string problems. A window is a range of elements in the array/string which usually defined by the start and end indices, i.e. [*i*,*j*) (left-closed, right-open). A sliding window is a window "slides" its two boundaries to the certain direction. For example, if we slide [*i*,*j*) to the right by 1 element, then it becomes [*i*+1,*j*+1) (left-closed, right-open).
>
> Back to our problem. We use HashSet to store the characters in current window [*i*,*j*) (*j*=*i* initially). Then we slide the index *j* to the right. If it is not in the HashSet, we slide *j*  further. Doing so until s[j] is already in the HashSet. At this point, we found the maximum size of substrings without duplicate characters start with index *i*. If we do this for all *i*, we get our answer.

> **Complexity Analysis**
>
> - Time complexity : O*(2*n*)=*O*(*n). In the worst case each character will be visited twice by *i* and *j*.
> - Space complexity : O*(*min*(*m*,*n*)). Same as the previous approach. We need *O*(*k*)space for the sliding window, where k*k* is the size of the `Set`. The size of the Set is upper bounded by the size of the string *n* and the size of the charset/alphabet m*m*. 





+ 使用set 存储滑窗中的元素，左闭右开，初始化i = 0, j = 0
+ 试探将j 放入集合时是否重复，不重复则将j 加入集合，并且j++, 用滑窗大小`j-i` 更新ans
+ 若重复则，i++

思考：

+ 之所以变快是因为 start index 后移后，end index没有动（没有再从 i ~ j 便利一遍）
+ 可是 start index 每次只向后移动1位

```c++
class Solution {
public:
    int lengthOfLongestSubstring(string s) {
        set<char> st;
        int n = s.size();
        int ans = 0, i = 0, j = 0;
        while (i < n && j < n) {
            if (!st.count(s[j])) {
                st.insert(s[j++]);
                ans = max(ans, j - i);
            } else {
                st.erase(s[i++]);
            }
        }
        return ans;
    }
};
```



```c++
class Solution {
public:
    int lengthOfLongestSubstring(string s) {
        int n = s.size(), ans = 0;
        map<char, int> mp;
        for (int i = 0, j = 0; j < n; j++) {
            if (mp.count(s[j])) {
                i = max(i, mp[s[j]] + 1); //s[j] 可能出现在 i 之前，这时不用考虑，i不变
            }
            ans = max(ans, j - i + 1);
            mp[s[j]] = j;
        }
        return ans;
    }
};
```

