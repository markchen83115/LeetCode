# Leetcode - 1593. Split a String Into the Max Number of Unique Substrings (M)

[Leetcode](https://leetcode.com/problems/split-a-string-into-the-max-number-of-unique-substrings/)

Given a string `s`, return _the maximum number of unique substrings that the given string can be split into_.

You can split string `s` into any list of **non-empty substrings**, where the concatenation of the substrings forms the original string. However, you must split the substrings such that all of them are **unique**.

A **substring** is a contiguous sequence of characters within a string.

**Example 1:**

Input: s = "ababccc"
Output: 5
Explanation: One way to split maximally is ['a', 'b', 'ab', 'c', 'cc']. Splitting like ['a', 'b', 'a', 'b', 'c', 'cc'] is not valid as you have 'a' and 'b' multiple times.

**Example 2:**

Input: s = "aba"
Output: 2
Explanation: One way to split maximally is ['a', 'ba'].

**Example 3:**

Input: s = "aa"
Output: 1
Explanation: It is impossible to split the string any further.

**Constraints:**

-   `1 <= s.length <= 16`
    
-   `s` contains only lower case English letters.

---
```java
// Java 30ms(Beats 86.27%), Time O(N * 2^N), Space O(N)
class Solution {
    int ret = 0;
    public int maxUniqueSplit(String s) {
        backtracking(s, 0, new HashSet<String>());
        return ret;
    }

    void backtracking(String s, int i, HashSet<String> seen)
    {
        if (i == s.length())
        {
            ret = Math.max(ret, seen.size());
            return;
        }

        for (int k = i + 1; k <= s.length(); k++)
        {
            String nextStr = s.substring(i, k);
            if (seen.contains(nextStr))
                continue;
            seen.add(nextStr);
            backtracking(s, k, seen);
            seen.remove(nextStr);
        }
    }
}
```
---

此題只能暴力解
使用backtracking計算出所有的可能, 並在過程中剪枝

###### tags: `Leetcode` `DFS` `search in an array`