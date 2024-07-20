# Leetcode - 336. Palindrome Pairs (H-)

[Leetcode](https://leetcode.com/problems/palindrome-pairs/)

You are given a **0-indexed** array of **unique** strings `words`.

A **palindrome pair** is a pair of integers `(i, j)` such that:

-   `0 <= i, j < word.length`,
-   `i != j`, and
-   `words[i] + words[j]` (the concatenation of the two strings) is a
-   palindrome
-   .

Return _an array of all the _**_palindrome pairs_**_ of _`words`.

**Example 1:**
```
Input: words = ["abcd","dcba","lls","s","sssll"]  
Output: [[0,1],[1,0],[3,2],[2,4]]  
Explanation: The palindromes are ["abcddcba","dcbaabcd","slls","llssssll"]
```
**Example 2:**
```
Input: words = ["bat","tab","cat"]  
Output: [[0,1],[1,0]]  
Explanation: The palindromes are ["battab","tabbat"]
```
**Example 3:**
```
Input: words = ["a",""]  
Output: [[0,1],[1,0]]  
Explanation: The palindromes are ["a","a"]
```
**Constraints:**

-   `1 <= words.length <= 5000`
-   `0 <= words[i].length <= 300`
-   `words[i]` consists of lowercase English letters.

---

```java
// Java 120ms(Beats 90.96%), Time O(N^2), Space O(N)
class Solution {
    public List<List<Integer>> palindromePairs(String[] words) {
        List<List<Integer>> ret = new LinkedList<>();
        Map<String, Integer> map = new HashMap<>();
        Set<Integer> lengthSet = new TreeSet<>();
        for (int i = 0; i < words.length; i++)
        {
            map.put(words[i], i);
            lengthSet.add(words[i].length());
        }

        for (int i = 0; i < words.length; i++)
        {
            // reverse entire word
            StringBuilder sb = new StringBuilder(words[i]);
            String reverse = sb.reverse().toString();
            if (map.containsKey(reverse) && i != map.get(reverse))
                ret.add(Arrays.asList(i, map.get(reverse)));

            // reverse partial word
            // ex: ssall -> llass (reverse)
            int n = reverse.length();
            for (int k : lengthSet)
            {
                if (k == n)
                    break;
                
                // front "ll"aass
                if (isPalindrome(reverse, 0, n - 1 - k))
                {
                    String partial = reverse.substring(n - k, n);
                    if (map.containsKey(partial))
                        ret.add(Arrays.asList(i, map.get(partial)));
                }

                // back llaa"ss"
                if (isPalindrome(reverse, k, n - 1))
                {
                    String partial = reverse.substring(0, k);
                    if (map.containsKey(partial))
                        ret.add(Arrays.asList(map.get(partial), i));
                }
            }
        }

        return ret;
    }

    boolean isPalindrome(String s, int i, int j)
    {
        while (i < j)
            if (s.charAt(i++) != s.charAt(j--))
                return false;
            
        return true;
    }
}
```

```csharp
// C#  
public class Solution {  
    public IList<IList<int>> PalindromePairs(string[] words) {  
        List<IList<int>> ret = new List<IList<int>>();  
        Dictionary<string, int> dict = new Dictionary<string, int>();  
        SortedSet<int> set = new SortedSet<int>();  
        for (int i = 0; i < words.Length; i++) {  
            dict.Add(words[i], i);  
            set.Add(words[i].Length);  
        }  
  
        for (int i = 0; i < words.Length; i++) {  
            // reverse all  
            string reverse = new string(string.Join("", words[i]).Reverse().ToArray());  
            if (dict.ContainsKey(reverse) && dict[reverse] != i) {  
                ret.Add(new List<int>() {i, dict[reverse]});  
            }  
            // partial  
            foreach (int k in set) {  
                int length = reverse.Length;  
                if (length == k)  
                    break;  
                // llsss <- sssll -> llsss  
                // forward  
                if (isPalindrome(reverse, 0, length - 1 - k)) {  
                    string str1 = reverse.Substring(length - k, k);  
                    if (dict.ContainsKey(str1)) {  
                        ret.Add(new List<int>() {i, dict[str1]});  
                    }  
                }  
                // backward  
                if (isPalindrome(reverse, k, length - 1)) {  
                    String str2 = reverse.Substring(0, k);  
                    if (dict.ContainsKey(str2)) {  
                        ret.Add(new List<int>() {dict[str2], i});  
                    }  
                }  
            }  
        }  
        return ret;  
    }  
  
    public bool isPalindrome(String str, int i, int j) {  
        while (i < j) {  
            if (str[i++] != str[j--])   
                return false;  
        }  
        return true;  
    }  
}
```
					 
---
					 
					 
> **Concept**

There are 2 words: `xxxxx`, `yyyyyyyy`  
find palindrome => `xxxxx **yyy** yyyyy`  
if middle `**yyy**` is palindrome, then `xxxxx` is reverse of `yyyyy`


###### tags: `Leetcode` `String`