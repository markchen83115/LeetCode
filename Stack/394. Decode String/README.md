# Leetcode - 394. Decode String (M)

[Leetcode](https://leetcode.com/problems/decode-string/description/)

Given an encoded string, return its decoded string.

The encoding rule is: `k[encoded_string]`, where the `encoded_string` inside the square brackets is being repeated exactly `k` times. Note that `k` is guaranteed to be a positive integer.

You may assume that the input string is always valid; there are no extra white spaces, square brackets are well-formed, etc. Furthermore, you may assume that the original data does not contain any digits and that digits are only for those repeat numbers, `k`. For example, there will not be input like `3a` or `2[4]`.

The test cases are generated so that the length of the output will never exceed `105`.

**Example 1:**
```
Input: s = "3[a]2[bc]"
Output: "aaabcbc"
```
**Example 2:**
```
Input: s = "3[a2[c]]"
Output: "accaccacc"
```
**Example 3:**
```
Input: s = "2[abc]3[cd]ef"
Output: "abcabccdcdcdef"
```
**Constraints:**

-   `1 <= s.length <= 30`
-   `s` consists of lowercase English letters, digits, and square brackets `'[]'`.
-   `s` is guaranteed to be **a valid** input.
-   All the integers in `s` are in the range `[1, 300]`.

---
```java
// Java 5ms(27.7%), Time O(N), Space O(N)
class Solution {
    public String decodeString(String s) {
        ArrayDeque<String> resStack = new ArrayDeque<>();
        ArrayDeque<Integer> countStack = new ArrayDeque<>();

        String ret = "";
        int idx = 0;
        while (idx < s.length()) {
            if (Character.isDigit(s.charAt(idx))) {
                int k = 0;
                while (Character.isDigit(s.charAt(idx))) {
                    k = k * 10 + (s.charAt(idx) - '0');
                    idx++;
                }
                countStack.push(k);
            } else if (s.charAt(idx) == '[') {
                resStack.push(ret);
                ret = "";
                idx++;
            } else if (s.charAt(idx) == ']') {
                int times = countStack.pop();
                StringBuilder sb = new StringBuilder(ret);
                for (int i = 1; i < times; i++)
                    sb.append(ret);
                ret = sb.toString();
                ret = resStack.pop() + ret;
                idx++;
            } else {
                ret += s.charAt(idx++);
            }
        }
        return ret;
    }
}
```

```java
// Java 1ms(78.16%), Time O(N), Space O(N)
class Solution {
    public String decodeString(String s) {
        ArrayDeque<StringBuilder> resStack = new ArrayDeque<>();
        ArrayDeque<Integer> countStack = new ArrayDeque<>();

        StringBuilder ret = new StringBuilder();
        int k = 0;
        for (char c : s.toCharArray()) {
            if (Character.isDigit(c)) {
                k = k * 10 + (c - '0');
            } else if (c == '[') {
                countStack.push(k);
                resStack.push(ret);
                ret = new StringBuilder();
                k = 0;
            } else if (c == ']') {
                int times = countStack.pop();
                StringBuilder tmp = ret;
                ret = resStack.pop();
                for (int i = 0; i < times; i++)
                    ret.append(tmp);
            } else {
                ret.append(c);
            }
        }
        return ret.toString();
    }
}
```

---

分成兩個stack, 一個存數字, 一個存字串,
若看見`數字`, 則計算數字為多少並放入`數字stack`,
若看見`[`, 則代表將目前計算結果存入`字串stack`, 將開始新的計算,
若看見`]`, 則代表此輪計算結束, 並且將`前次結果`與`數字*本次結果`合併, 
`當前結果 = 前次結果 + 數字 * 本次結果`

###### tags: `Leetcode` `Stack`