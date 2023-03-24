# Leetcode - 844. Backspace String Compare (E)

[Leetcode](https://leetcode.com/problems/backspace-string-compare/description/)

Given two strings `s` and `t`, return `true` _if they are equal when both are typed into empty text editors_. `'#'` means a backspace character.

Note that after backspacing an empty text, the text will continue empty.

**Example 1:**
```
Input: s = "ab#c", t = "ad#c"
Output: true
Explanation: Both s and t become "ac".
```
**Example 2:**
```
Input: s = "ab##", t = "c#d#"
Output: true
Explanation: Both s and t become "".
```
**Example 3:**
```
Input: s = "a#c", t = "b"
Output: false
Explanation: s becomes "c" while t becomes "b".
```
**Constraints:**

-   `1 <= s.length, t.length <= 200`
-   `s` and `t` only contain lowercase letters and `'#'` characters.

**Follow up:** Can you solve it in `O(n)` time and `O(1)` space?

---
```java
// Java 2ms, Time O(N), Space O(N)
class Solution {
    public boolean backspaceCompare(String s, String t) {
        ArrayDeque<Character> stack1 = new ArrayDeque<>();
        ArrayDeque<Character> stack2 = new ArrayDeque<>();
        for (char c : s.toCharArray()) {
            if (c != '#') {
                stack1.push(c);
            } else if (!stack1.isEmpty()) {
                stack1.pop();
            }
        }

        for (char c : t.toCharArray()) {
            if (c != '#') {
                stack2.push(c);
            } else if (!stack2.isEmpty()) {
                stack2.pop();
            }
        }

        if (stack1.size() != stack2.size())
            return false;
        while (!stack1.isEmpty()) {
            if (stack1.pop() != stack2.pop())
                return false;
        }

        return true;
    }
}
```
```java
// Java 0ms, Time O(N), Space O(1)
class Solution {
    public boolean backspaceCompare(String s, String t) {
        int i = s.length() - 1;
        int j = t.length() - 1;
        int back = 0; 
        while (true) {
            while (i >= 0 && (back > 0 || s.charAt(i) == '#')) {
                back += s.charAt(i) == '#' ? 1 : -1;
                i--;
            }
            back = 0;
            while (j >= 0 && (back > 0 || t.charAt(j) == '#')) {
                back += t.charAt(j) == '#' ? 1 : -1;
                j--;
            }
            back = 0;
            if (i >= 0 && j >= 0 && s.charAt(i) == t.charAt(j)) {
                i--;
                j--;
            } else {
                break;
            }
        }

        return i == -1 && j == -1;
    }
}
```

---

###### tags: `Leetcode` `Stack`