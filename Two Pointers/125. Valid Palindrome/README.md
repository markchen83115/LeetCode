# Leetcode - 125. Valid Palindrome(E)

[Leetcode](https://leetcode.com/problems/valid-palindrome/)

A phrase is a **palindrome** if, after converting all uppercase letters into lowercase letters and removing all non-alphanumeric characters, it reads the same forward and backward. Alphanumeric characters include letters and numbers.

Given a string `s`, return `true`_ if it is a _**_palindrome_**_, or _`false`_ otherwise_.

**Example 1:**
```
Input: s = "A man, a plan, a canal: Panama"  
Output: true  
Explanation: "amanaplanacanalpanama" is a palindrome.
```
**Example 2:**
```
Input: s = "race a car"  
Output: false  
Explanation: "raceacar" is not a palindrome.
```
**Example 3:**
```
Input: s = " "  
Output: true  
Explanation: s is an empty string "" after removing non-alphanumeric characters.  
Since an empty string reads the same forward and backward, it is a palindrome.
```
**Constraints:**

-   `1 <= s.length <= 2 * 105`
-   `s` consists only of printable ASCII characters.

---

```java
// Java  
class Solution {  
    public boolean isPalindrome(String s) {  
        char[] array = s.toCharArray();  
        int l = 0, r = array.length - 1;  
        while (l < r) {  
            while (l < r && !Character.isLetterOrDigit(array[l])) {  
                l++;  
            }  
            while (l < r && !Character.isLetterOrDigit(array[r])) {  
                r--;  
            }  
            if (l < r && Character.toLowerCase(array[l]) != Character.toLowerCase(array[r])) {  
                return false;  
            }  
            l++;  
            r--;  
        }  
        return true;  
    }  
}
```

```csharp
// C#  
public class Solution {  
    public bool IsPalindrome(string s) {  
        int l = 0, r = s.Length - 1;  
        while (l < r) {  
            while (l < r && !Char.IsLetterOrDigit(s[l])) {  
                l++;  
            }  
            while (l < r && !Char.IsLetterOrDigit(s[r])) {  
                r--;  
            }  
            if (Char.ToLower(s[l]) != Char.ToLower(s[r])) {  
                return false;  
            }  
            l++;  
            r--;  
        }  
        return true;  
    }  
}
```

學習使用two pointer去比對迴文  
以及`Character.isLetterOrDigit()`驗證是否為英數字



###### tags: `Leetcode` `Two Pointers`