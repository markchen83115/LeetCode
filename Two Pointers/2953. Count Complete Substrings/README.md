# Leetcode - 2953. Count Complete Substrings (H)

[Leetcode](https://leetcode.com/problems/count-complete-substrings/)

You are given a string `word` and an integer `k`.

A substring `s` of `word` is **complete** if:

-   Each character in `s` occurs **exactly** `k` times.
-   The difference between two adjacent characters is **at most** `2`. That is, for any two adjacent characters `c1` and `c2` in `s`, the absolute difference in their positions in the alphabet is **at most** `2`.

Return _the number of **complete **substrings of_ `word`.

A **substring** is a **non-empty** contiguous sequence of characters in a string.

**Example 1:**

> **Input:** word = "igigee", k = 2
> **Output:** 3
> **Explanation:** The complete substrings where each character appears exactly twice and the difference between adjacent characters is at most 2 are: **igig**ee, igig**ee**, **igigee**.

**Example 2:**

> **Input:** word = "aaabbbccc", k = 3
> **Output:** 6
> **Explanation:** The complete substrings where each character appears exactly three times and the difference between adjacent characters is at most 2 are: **aaa**bbbccc, aaa**bbb**ccc, aaabbb**ccc**, **aaabbb**ccc, aaa**bbbccc**, **aaabbbccc**.

**Constraints:**

-   `1 <= word.length <= 105`
-   `word` consists only of lowercase English letters.
-   `1 <= k <= word.length`

---
```java
// Java 99ms(Beats 84.21%), Time O(26*N), Space O(26)
class Solution {
    public int countCompleteSubstrings(String word, int k) {
        int n = word.length();
        int ret = 0;
        for (int i = 0; i < n; )
        {
            int j = i + 1;
            while (j < n && Math.abs(word.charAt(j) - word.charAt(j-1)) <= 2)
                j++;
            
            ret += help(word.substring(i, j), k);
            i = j;
        }
        return ret;
    }

    int help(String s, int k)
    {
        int ret = 0;
        int n = s.length();
        HashSet<Character> set = new HashSet<>();
        for (char ch : s.toCharArray())
            set.add(ch);

        for (int i = 1; i <= set.size(); i++)
        {
            int len = k * i;
            int[] freq = new int[26];
            int count = 0;
            for (int j = 0; j < n; j++)
            {
                if (++freq[s.charAt(j) - 'a'] == k)
                    count++;

                if (j >= len && --freq[s.charAt(j - len) - 'a'] == k - 1)
                    count--;
                
                if (j >= len - 1 && i == count)
                    ret++;
            }
        }
        return ret;
    }
}
```
---

參考[【每日一题】LeetCode 2953. Count Complete Substrings - YouTube](https://youtu.be/4DYbP4shsno)

第一步, 將原字串切割成多個區間, 只考慮那些「相鄰字元大小不超過2」的那些區間

第二步, 計算每個區間裡，再有多少個符合條件的`substring`, 也就是要求`substring`裡出現的字元的頻次都是k
注意到字符的種類只有`26`種，如果只出現`一種字符`，那長度就是`k`；如果出現`兩種字符`，那長度就是`2k`，以此類推
我們發現可以遍歷出現字符的種類數目，然後用一個固定長度的滑窗來判定是否存在`substring`符合條件


###### tags: `Leetcode` `Two Pointers` `Sliding window`