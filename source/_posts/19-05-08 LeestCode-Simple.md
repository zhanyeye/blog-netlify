---
title: 10-05-08 LeestCode-Simple
date: 2019-05-08 21:21:58
tags:
categories:
- LeetCode
---



###### Valid Parentheses

Easy

Given a string containing just the characters `'('`, `')'`, `'{'`, `'}'`, `'['` and `']'`, determine if the input string is valid.

An input string is valid if:

1. Open brackets must be closed by the same type of brackets.
2. Open brackets must be closed in the correct order.

Note that an empty string is also considered valid.

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

