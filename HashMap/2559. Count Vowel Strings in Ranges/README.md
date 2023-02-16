# Leetcode - 2559. Count Vowel Strings in Ranges (M-)

[Leetcode](https://leetcode.com/problems/count-vowel-strings-in-ranges/description/)

You are given a **0-indexed** array of strings `words` and a 2D array of integers `queries`.

Each query `queries[i] = [li, ri]` asks us to find the number of strings present in the range `li` to `ri` (both **inclusive**) of `words` that start and end with a vowel.

Return an array `ans` of size `queries.length`, where `ans[i]` is the answer to the `i`th query.

**Note** that the vowel letters are `'a'`, `'e'`, `'i'`, `'o'`, and `'u'`.

**Example 1:**
```
Input: words = ["aba","bcb","ece","aa","e"], queries = [[0,2],[1,4],[1,1]]
Output: [2,3,0]
Explanation: The strings starting and ending with a vowel are "aba", "ece", "aa" and "e".
The answer to the query [0,2] is 2 (strings "aba" and "ece").
to query [1,4] is 3 (strings "ece", "aa", "e").
to query [1,1] is 0.
We return [2,3,0].
```
**Example 2:**
```
Input: words = ["a","e","i"], queries = [[0,2],[0,1],[2,2]]
Output: [3,2,1]
Explanation: Every string satisfies the conditions, so we return [3,2,1].
```
**Constraints:**

-   `1 <= words.length <= 105`
-   `1 <= words[i].length <= 40`
-   `words[i]` consists only of lowercase English letters.
-   `sum(words[i].length) <= 3 * 105`
-   `1 <= queries.length <= 105`
-   `0 <= li <= ri < words.length`

---
```java
// Java
class Solution {
    public int[] vowelStrings(String[] words, int[][] queries) {
        int n = words.length;
        int[] prefix = new int[n+1];
        HashSet<Character> vowel = new HashSet<>();
        int sum = 0;
        int[] ret = new int[queries.length];
        
        prefix[0] = 0;
        vowel.add('a');
        vowel.add('e');
        vowel.add('i');
        vowel.add('o');
        vowel.add('u');
        
        for (int i = 0; i < n; i++) {
            if (vowel.contains(words[i].charAt(0)) && vowel.contains(words[i].charAt(words[i].length() - 1))) {
                sum++;
            }
            prefix[i+1] = sum;
        }
        
        for (int i = 0; i < queries.length; i++) {
            int a = queries[i][0], b = queries[i][1];
            ret[i] = prefix[b+1] - prefix[a];
        }
        
        return ret;
    }
}
```

---

利用prefix sum算出個線段總和
計算區間`[i, j]`時, 直接取用`prefix[j+1] - prefix[i]`

注意: `prefix[0] = 0` 代表沒有任何數字時和為0, 
意同`prefix[-1] = 0`, 但使用array無法使用`index = -1`


###### tags: `Leetcode` `HashMap` `Hash+Prefix`
