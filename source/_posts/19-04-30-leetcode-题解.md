---
title: leetcode 题解
date: 2019-04-30 17:32:39
tags:
categories:
- LeetCode
---



###### Two Sum

Easy

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

