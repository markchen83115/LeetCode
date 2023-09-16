# Leetcode - 1647. Minimum Deletions to Make Character Frequencies Unique (M-)

[Leetcode](https://leetcode.com/problems/minimum-deletions-to-make-character-frequencies-unique/)

A string `s` is called **good** if there are no two different characters in `s` that have the same **frequency**.

Given a string `s`, return_ the **minimum** number of characters you need to delete to make _`s`_ **good**._

The **frequency** of a character in a string is the number of times it appears in the string. For example, in the string `"aab"`, the **frequency** of `'a'` is `2`, while the **frequency** of `'b'` is `1`.

**Example 1:**
```
Input: s = "aab"
Output: 0
Explanation: `s` is already good.
```
**Example 2:**
```
Input: s = "aaabbbcc"
Output: 2
Explanation: You can delete two 'b's resulting in the good string "aaabcc".
Another way it to delete one 'b' and one 'c' resulting in the good string "aaabbc".
```
**Example 3:**
```
Input: s = "ceabaacb"
Output: 2
Explanation: You can delete both 'c's resulting in the good string "eabaab".
Note that we only care about characters that are still in the string at the end (i.e. frequency of 0 is ignored).
```
**Constraints:**

-   `1 <= s.length <= 105`
-   `s` contains only lowercase English letters.

---
```java
// Java 7ms (Beats 96.95%), Time O(N^2), Space O(N)
class Solution {
    public int minDeletions(String s) {
        int[] freqs = new int[26];
        int ret = 0;
        Set<Integer> used = new HashSet<>();
        for (char c : s.toCharArray())
            freqs[c - 'a']++;
        
        for (int i = 0; i < 26; i++)
        {
            int count = freqs[i];
            while (count > 0 && used.contains(count)) 
            {
                count--;
                ret++;
            }
            used.add(count);
        }

        return ret;
    }
}
```
```csharp
// C# 7ms (Beats 96.95%), Time O(N^2), Space O(N)
public class Solution {
    public int MinDeletions(string s) {
        int ret = 0;
        int[] freqs = new int[26];
        HashSet<int> used = new HashSet<int>();
        foreach (char c in s)
            freqs[c - 'a']++;
        
        for (int i = 0; i < 26; i++)
        {
            int freq = freqs[i];
            while (freq > 0 && used.Contains(freq))
            {
                freq--;
                ret++;
            }
            used.Add(freq);
        }

        return ret;
    }
}
```
---

###### tags: `Leetcode` `HashMap`