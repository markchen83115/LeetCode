# Leetcode - 8. String to Integer (atoi) (M)

[Leetcode](https://leetcode.com/problems/string-to-integer-atoi/)

Implement the `myAtoi(string s)` function, which converts a string to a 32-bit signed integer (similar to C/C++'s `atoi` function).

The algorithm for `myAtoi(string s)` is as follows:

1.  Read in and ignore any leading whitespace.
2.  Check if the next character (if not already at the end of the string) is `'-'` or `'+'`. Read this character in if it is either. This determines if the final result is negative or positive respectively. Assume the result is positive if neither is present.
3.  Read in next the characters until the next non-digit character or the end of the input is reached. The rest of the string is ignored.
4.  Convert these digits into an integer (i.e. `"123" -> 123`, `"0032" -> 32`). If no digits were read, then the integer is `0`. Change the sign as necessary (from step 2).
5.  If the integer is out of the 32-bit signed integer range `[-231, 231 - 1]`, then clamp the integer so that it remains in the range. Specifically, integers less than `-231` should be clamped to `-231`, and integers greater than `231 - 1` should be clamped to `231 - 1`.
6.  Return the integer as the final result.

**Note:**

-   Only the space character `' '` is considered a whitespace character.
-   **Do not ignore** any characters other than the leading whitespace or the rest of the string after the digits.

**Example 1:**
```
Input: s = "42"  
Output: 42  
Explanation: The underlined characters are what is read in, the caret is the current reader position.  
Step 1: "42" (no characters read because there is no leading whitespace)  
         ^  
Step 2: "42" (no characters read because there is neither a '-' nor '+')  
         ^  
Step 3: "42" ("42" is read in)  
           ^  
The parsed integer is 42.  
Since 42 is in the range [-231, 231 - 1], the final result is 42.
```
**Example 2:**
```
Input: s = "   -42"  
Output: -42  
Explanation:  
Step 1: "   -42" (leading whitespace is read and ignored)  
            ^  
Step 2: "   -42" ('-' is read, so the result should be negative)  
             ^  
Step 3: "   -42" ("42" is read in)  
               ^  
The parsed integer is -42.  
Since -42 is in the range [-231, 231 - 1], the final result is -42.
```
**Example 3:**
```
Input: s = "4193 with words"  
Output: 4193  
Explanation:  
Step 1: "4193 with words" (no characters read because there is no leading whitespace)  
         ^  
Step 2: "4193 with words" (no characters read because there is neither a '-' nor '+')  
         ^  
Step 3: "4193 with words" ("4193" is read in; reading stops because the next character is a non-digit)  
             ^  
The parsed integer is 4193.  
Since 4193 is in the range [-231, 231 - 1], the final result is 4193.
```
**Constraints:**

-   `0 <= s.length <= 200`
-   `s` consists of English letters (lower-case and upper-case), digits (`0-9`), `' '`, `'+'`, `'-'`, and `'.'`.

---

```java
// Java  
class Solution {  
    public int myAtoi(String s) {  
        int index = 0;  
        int len = s.length();  
        int sign = 1;  
        int result = 0;  
        // Check empty string  
        if (len == 0) {  
            return 0;  
        }  
  
        // handle space  
        while (index < len && s.charAt(index) == ' ') {  
            index++;  
        }  
  
        // + -  
        if (index < len && (s.charAt(index) == '+' || s.charAt(index) == '-')) {  
            sign = s.charAt(index) == '+' ? 1 : -1;  
            index++;  
        }  
  
        // convert to the actual number and make sure it's not overflow  
        while (index < len) {  
            int digit = s.charAt(index) - '0';  
            if (digit < 0 || digit > 9) {  
                break;  
            }  
  
            // check overflow  
            if (result > Integer.MAX_VALUE / 10   
              || (result == Integer.MAX_VALUE / 10 && digit > Integer.MAX_VALUE % 10)) {  
                  return sign == 1 ? Integer.MAX_VALUE : Integer.MIN_VALUE;  
            }  
  
            result = result * 10 + digit;  
            index++; // don't forget to increment the counter  
        }  
  
        return result * sign;  
    }  
}
```

```java
// Java (better if condition)  
class Solution {  
    public int myAtoi(String s) {  
        int index = 0;  
        int len = s.length();  
        int sign = 1;  
        int result = 0;  
        // Check empty string  
        if (len == 0) {  
            return 0;  
        }  
  
        // handle space  
        while (index < len && s.charAt(index) == ' ') {  
            index++;  
        }  
  
        // + -  
        if (index < len && (s.charAt(index) == '+' || s.charAt(index) == '-')) {  
            sign = s.charAt(index) == '+' ? 1 : -1;  
            index++;  
        }  
  
        // convert to the actual number and make sure it's not overflow  
        while (index < len) {  
            int digit = s.charAt(index) - '0';  
            if (digit < 0 || digit > 9) {  
                break;  
            }  
  
            // check overflow  
            // if (result > Integer.MAX_VALUE / 10   
            //     || (result == Integer.MAX_VALUE / 10 && digit > Integer.MAX_VALUE % 10)) {  
            //         return sign == 1 ? Integer.MAX_VALUE : Integer.MIN_VALUE;  
            // }  
            if (result >  (Integer.MAX_VALUE - digit) / 10) {  
                return sign == 1 ? Integer.MAX_VALUE : Integer.MIN_VALUE;  
            }  
            //result * 10 + digit > 2147483647  
  
            result = result * 10 + digit;  
            index++; // don't forget to increment the counter  
        }  
  
        return result * sign;  
    }  
}
```

---
```java
// check overflow  
if (result > Integer.MAX_VALUE / 10   
    || (result == Integer.MAX_VALUE / 10 && digit > Integer.MAX_VALUE % 10)) {  
        return sign == 1 ? Integer.MAX_VALUE : Integer.MIN_VALUE;  
}
```

`Integer.MAX_VALUE = 2147483647 (10³² — 1)`  
`Integer.MIN_VALUE = -2147483648 (-10³²)`  
`2147483647 / 10 = 214748364,`  
若`result = 21474836**5**`, 就算後面加上最小的`digit **0**`,  
依然`21474836**50** > 2147483647` overflow  
因此`result > Integer.MAX_VALUE / 10`

當`result = 214748364`時, 則需檢查`digit > 7 ?`  
否則會overflow

> 優化

因為需要檢查`result * 10 + digit > 2147483647` 是否會overflow  
因此變換為`**result > (Integer.MAX_VALUE - digit) / 10**`



###### tags: `Leetcode` `String`
