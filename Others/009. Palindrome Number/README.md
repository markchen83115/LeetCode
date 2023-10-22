# Leetcode - 9. Palindrome Number (E)

[Leetcode](https://leetcode.com/problems/palindrome-number/)

Given an integer `x`, return _`true`_ if `x` is a _`palindrome`_, and _`false`_ otherwise.

**Example 1:**
```
Input: x = 121
Output: true
Explanation: 121 reads as 121 from left to right and from right to left.
```
**Example 2:**
```
Input: x = -121
Output: false
Explanation: From left to right, it reads -121. From right to left, it becomes 121-. Therefore it is not a palindrome.
```
**Example 3:**
```
Input: x = 10
Output: false
Explanation: Reads 01 from right to left. Therefore it is not a palindrome.
```
**Constraints:**

-   `-231 <= x <= 231 - 1`

**Follow up:** Could you solve it without converting the integer to a string?

---
```java
// Java 4ms(Beats 100%), Time O(N), Space O(1)
class Solution {
    public boolean isPalindrome(int x) {
        if (x < 0)
            return false;
        
        int y = x;
        int reverse = 0;
        while (y != 0)
        {
            reverse = reverse * 10 + y % 10;
            y /= 10;
        }

        return x == reverse;
    }
}
```

---

###### tags: `Leetcode` `Others`