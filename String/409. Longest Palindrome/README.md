# Leetcode - 409. Longest Palindrome (E)

[Leetcode](https://leetcode.com/problems/longest-palindrome/)

Given a string `s` which consists of lowercase or uppercase letters, return _the length of the _**_longest palindrome_** that can be built with those letters.

Letters are **case sensitive**, for example, `"Aa"` is not considered a palindrome here.

**Example 1:**
```
Input: s = "abccccdd"  
Output: 7  
Explanation: One longest palindrome that can be built is "dccaccd", whose length is 7.
```
**Example 2:**
```
Input: s = "a"  
Output: 1  
Explanation: The longest palindrome that can be built is "a", whose length is 1.
```
**Constraints:**

-   `1 <= s.length <= 2000`
-   `s` consists of lowercase **and/or** uppercase English letters only.

---

```java
// Java  
// HashSet  
class Solution {  
    public int longestPalindrome(String s) {  
        HashSet set = new HashSet();  
        int maxLen = 0;  
        char[] array = s.toCharArray();  
        for (char c : array) {  
            if (set.contains(c)) {  
                maxLen += 2;  
                set.remove(c);  
            } else {  
                set.add(c);  
            }  
        }  
        if (set.size() > 0) {  
            maxLen++;  
        }  
        return maxLen;  
    }  
}
```

```java
// Java  
// Array  
class Solution {  
    public int longestPalindrome(String s) {  
        int maxLen = 0;  
        int[] freq = new int[58];  
        boolean isOdd = false;  
        for (char c : s.toCharArray()) {  
            freq[c - 'A']++;  
        }  
        for (int i = 0; i < 58; i++) {  
            maxLen += freq[i];  
            if (freq[i] % 2 != 0) {     //odd  
                maxLen--;  
                isOdd = true;  
            }  
        }  
        return isOdd ? maxLen + 1 : maxLen;  
    }  
}
```

---

> **字母個數**

偶數 → 可全部使用  
奇數 → **_個數=3 視為 2+1_**, 可使用一個在正中間 ex: aaabaaa

更快速的解: 利用題目只會是英文字母,  
因此利用[ASCII](https://zh.wikipedia.org/zh-tw/ASCII)的原因, 只開A到z的array空間,  
`122號(z) - 65號(A) + 1 = 58`

> **ASCII**

可顯示字元編號範圍是32–126（0x20–0x7E），共95個字元。  
32～126(共95個)是字元(32是空格)，其中48～57為0到9十個阿拉伯數字。  
**65～90為26個大寫英文字母，97～122號為26個小寫英文字母**，其餘為一些標點符號、運算符號等。



###### tags: `Leetcode` `String`
