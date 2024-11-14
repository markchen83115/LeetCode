# Leetcode - 3306. Count of Substrings Containing Every Vowel and K Consonants II (H-)

[Leetcode](https://leetcode.com/problems/count-of-substrings-containing-every-vowel-and-k-consonants-ii/)

You are given a string `word` and a **non-negative** integer `k`.

Return the total number of substrings of `word` that contain every vowel (`'a'`, `'e'`, `'i'`, `'o'`, and `'u'`) **at least** once and **exactly** `k` consonants.

**Example 1:**

> **Input:** word = "aeioqq", k = 1
> 
> **Output:** 0
> 
> **Explanation:**
> 
> There is no substring with every vowel.

**Example 2:**

> **Input:** word = "aeiou", k = 0
> 
> **Output:** 1
> 
> **Explanation:**
> 
> The only substring with every vowel and zero consonants is `word[0..4]`, which is `"aeiou"`.

**Example 3:**

> **Input:** word = "ieaouqqieaouqq", k = 1
> 
> **Output:** 3
> 
> **Explanation:**
> 
> The substrings with every vowel and one consonant are:
> 
> -   `word[0..5]`, which is `"ieaouq"`.
> -   `word[6..11]`, which is `"qieaou"`.
> -   `word[7..12]`, which is `"ieaouq"`.

**Constraints:**

-   `5 <= word.length <= 2 * 105`
-   `word` consists only of lowercase English letters.
-   `0 <= k <= word.length - 5`

---
```java
// Java 169ms(Beats 69.56%), Time O(N), Space O(N)
class Solution {
    public long countOfSubstrings(String word, int k) {
        HashSet<Character> set = new HashSet<>();
        set.add('a');
        set.add('e');
        set.add('i');
        set.add('o');
        set.add('u');

        int vowelCount = 5;
        int[] freq = new int[26];
        freq['a' - 'a']++;
        freq['e' - 'a']++;
        freq['i' - 'a']++;
        freq['o' - 'a']++;
        freq['u' - 'a']++;

        int n = word.length();
        // 預先處理word[i]之後有幾個母音 (不含i)
        // p a e t i o u q q
        //       2 2 1 0 0 0
        int[] vowelLen = new int[n];
        int len = 0;
        for (int i = n-1; i >= 0; i--)
        {
            vowelLen[i] = len;
            if (set.contains(word.charAt(i)))
                len++;
            else
                len = 0;
        }

        int j = 0;
        long ret = 0;
        
        for (int i = 0; i < n; i++)
        {
            while (j < n && (vowelCount > 0 || k > 0))
            {
                char ch = word.charAt(j);
                if (set.contains(ch))
                {
                    if (--freq[ch - 'a'] == 0)
                        vowelCount--;
                }
                else
                {
                    k--;
                }
                j++;
            }

            if (k == 0 && vowelCount == 0)
            {
                ret += vowelLen[j - 1] + 1;
            }
            
            
            char ch = word.charAt(i);
            if (set.contains(ch))
            {
                if (++freq[ch - 'a'] == 1)
                    vowelCount++;
            }
            else
            {
                k++;
            }
        }

        return ret;
    }
}       
```
---

利用雙指針找出最小範圍, 當`子音不足k個`需往右擴大, 而`沒有所有母音`也要往右擴大
可利用`freq[a, e, i, o ,u]`來處理, 
若`freq[]從1變為0` 代表缺少該母音
若`freq[]從0變為1` 代表擁有該母音

以及必須先預處理以word[i]為結尾時, i再往後有幾個母音
若在雙指針時才處理需要`Time O(N^2)`, 預先處理只需`Time O(N)`

###### tags: `Leetcode` `Two Pointers` `Sliding window : Distinct Characters`