# Leetcode - 030. Substring with Concatenation of All Words (H)

[Leetcode](https://leetcode.com/problems/substring-with-concatenation-of-all-words/)

You are given a string `s` and an array of strings `words`. All the strings of `words` are of **the same length**.

A **concatenated string** is a string that exactly contains all the strings of any permutation of `words` concatenated.

-   For example, if `words = ["ab","cd","ef"]`, then `"abcdef"`, `"abefcd"`, `"cdabef"`, `"cdefab"`, `"efabcd"`, and `"efcdab"` are all concatenated strings. `"acdbef"` is not a concatenated string because it is not the concatenation of any permutation of `words`.

Return an array of _the starting indices_ of all the concatenated substrings in `s`. You can return the answer in **any order**.

**Example 1:**

> **Input:** s = "barfoothefoobarman", words = ["foo","bar"]
> 
> **Output:** [0,9]
> 
> **Explanation:**
> 
> The substring starting at 0 is `"barfoo"`. It is the concatenation of `["bar","foo"]` which is a permutation of `words`.  
> The substring starting at 9 is `"foobar"`. It is the concatenation of `["foo","bar"]` which is a permutation of `words`.

**Example 2:**

> **Input:** s = "wordgoodgoodgoodbestword", words = ["word","good","best","word"]
> 
> **Output:** []
> 
> **Explanation:**
> 
> There is no concatenated substring.

**Example 3:**

> **Input:** s = "barfoofoobarthefoobarman", words = ["bar","foo","the"]
> 
> **Output:** [6,9,12]
> 
> **Explanation:**
> 
> The substring starting at 6 is `"foobarthe"`. It is the concatenation of `["foo","bar","the"]`.  
> The substring starting at 9 is `"barthefoo"`. It is the concatenation of `["bar","the","foo"]`.  
> The substring starting at 12 is `"thefoobar"`. It is the concatenation of `["the","foo","bar"]`.

**Constraints:**

-   `1 <= s.length <= 104`
-   `1 <= words.length <= 5000`
-   `1 <= words[i].length <= 30`
-   `s` and `words[i]` consist of lowercase English letters.

---
```java
// Java 19ms(Beats 25.70%), Time O(M*N), Space O(M)
class Solution {
    public List<Integer> findSubstring(String s, String[] words) {
        List<Integer> ret = new ArrayList<>();
        int n = s.length();
        int k = words.length;
        int m = words[0].length();
        HashMap<String, Integer> wordMap = new HashMap<>();
        int count = 0;
        for (String w : words)
        {
            wordMap.merge(w, 1, Integer::sum);
            if (wordMap.get(w) == 1)
                count++;
        }

        for (int p = 0; p < m; p++)
        {
            int need = count;
            HashMap<String, Integer> freqMap = new HashMap<>(wordMap); //copy HashMap
            for (int i = p; i + m <= n; i += m)
            {
                String t = s.substring(i, i + m);
                freqMap.merge(t, -1, Integer::sum);
                if (freqMap.get(t) == 0)
                    need--;
                
                if (i >= m * k)
                {
                    t = s.substring(i - m * k, i - m * k + m);
                    freqMap.merge(t, 1, Integer::sum);
                    if (freqMap.get(t) == 1)
                        need++;
                }

                if (need == 0)
                    ret.add(i - m * k + m);
            }
        }

        return ret;
    }
}

// Java 17ms(Beats 47.24%), Time O(N*M), Space O(M)
class Solution {
    public List<Integer> findSubstring(String s, String[] words) {
        List<Integer> ret = new ArrayList<>();
        int n = s.length();
        int m = words[0].length();
        HashMap<String, Integer> wordMap = new HashMap<>();
        for (String w : words)
            wordMap.merge(w, 1, Integer::sum);

        for (int start = 0; start < m; start++)
        {
            int i = start;
            int j = start;
            int count = 0;
            HashMap<String, Integer> map = new HashMap<>();

            while (j + m <= n)
            {
                String ss = s.substring(j, j + m);
                map.merge(ss, 1, Integer::sum);
                j += m;
                count++;
                
                if (count == words.length)
                {
                    if (map.equals(wordMap))
                        ret.add(i);

                    String t = s.substring(i, i + m);
                    map.computeIfPresent(t, (a, b) -> b > 1 ? b - 1 : null);
                    i += m;
                    count--;
                }
            }
        }

        return ret;
    }
}
```
---

典型的雙指針演算法問題
常規思路：右指針一路前進，遇到不符合條件的情況就移動左指針直至消除負面條件，再接著移動右指針。

增加一個外層循環，雙指標的起始點可以從0~M, M是每個字的長度. 注意count和showTime在每個start都要清零，故設定為循環內變數。
```java
        for (int start = 0; start < m; start++)
        {
            int i = start;
            int j = start;
            int count = 0;
            HashMap<String, Integer> map = new HashMap<>();
            
            while (j + m <= n)
            {
                ...
            }
        }
```

利用`HashMap.eqauls()`比對兩個`HashMap`是否相同
因為使用`HashMap.eqauls()`, 所以當左指針右移而清除掉`HashMap`裡面的東西時
若`(key, value)`的`value`已經是`0`, 記得要`remove key`
否則會影響`HashMap.eqauls()`比對

---

#### 如何copy HashMap
```java
myobjectListB = new HashMap<Integer,myObject>(myobjectListA);
```


###### tags: `Leetcode` `Two Pointers`