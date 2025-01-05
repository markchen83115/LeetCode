# Leetcode - 3412. Find Mirror Score of a String (M-)

[Leetcode](https://leetcode.com/problems/find-mirror-score-of-a-string/)

You are given a string `s`.

We define the **mirror** of a letter in the English alphabet as its corresponding letter when the alphabet is reversed. For example, the mirror of `'a'` is `'z'`, and the mirror of `'y'` is `'b'`.

Initially, all characters in the string `s` are **unmarked**.

You start with a score of 0, and you perform the following process on the string `s`:

-   Iterate through the string from left to right.
-   At each index `i`, find the closest **unmarked** index `j` such that `j < i` and `s[j]` is the mirror of `s[i]`. Then, **mark** both indices `i` and `j`, and add the value `i - j` to the total score.
-   If no such index `j` exists for the index `i`, move on to the next index without making any changes.

Return the total score at the end of the process.

**Example 1:**

> **Input:** s = "aczzx"
> 
> **Output:** 5
> 
> **Explanation:**
> 
> -   `i = 0`. There is no index `j` that satisfies the conditions, so we skip.
> -   `i = 1`. There is no index `j` that satisfies the conditions, so we skip.
> -   `i = 2`. The closest index `j` that satisfies the conditions is `j = 0`, so we mark both indices 0 and 2, and then add `2 - 0 = 2` to the score.
> -   `i = 3`. There is no index `j` that satisfies the conditions, so we skip.
> -   `i = 4`. The closest index `j` that satisfies the conditions is `j = 1`, so we mark both indices 1 and 4, and then add `4 - 1 = 3` to the score.

**Example 2:**

> **Input:** s = "abcdef"
> 
> **Output:** 0
> 
> **Explanation:**
> 
> For each index `i`, there is no index `j` that satisfies the conditions.

**Constraints:**

-   `1 <= s.length <= 105`
-   `s` consists only of lowercase English letters.

---
```java
//Java 12ms(Beats 100.00%), Time O(N), Space O(N)
class Solution {
    public long calculateScore(String s) {
        long ret = 0;
        List<Integer>[] freq = new ArrayList[26];
        for (int i = 0; i < 26; i++)
            freq[i] = new ArrayList<>();
        
        for (int i = 0; i < s.length(); i++)
        {
            char ch = s.charAt(i);
            char mirror = (char) ('a' + ('z' - ch));
            List<Integer> list = freq[mirror - 'a'];
            if (list.size() > 0)
            {
                ret += i - list.get(list.size() - 1);
                list.remove(list.size() - 1);
            }
            else
                freq[ch - 'a'].add(i);
        }
        
        return ret;
    }
} 
```
---

依照題目的要求, 從頭到尾遍歷s
`HashMap`內尋找`s[i]`的`mirror list`是否有值, 有個話取`list`最後一個使用

若沒使用`s[i]`的話, 則將`s[i]`放入`HashMap`內
因為題目要求要使用最近的`index`, 因此是放在`list`的最後一個, 方便下次使用


###### tags: `Leetcode` `HashMap`