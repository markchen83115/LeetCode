# Leetcode - 1963. Minimum Number of Swaps to Make the String Balanced (M+)

[Leetcode](https://leetcode.com/problems/minimum-number-of-swaps-to-make-the-string-balanced/)

You are given a **0-indexed** string `s` of **even** length `n`. The string consists of **exactly** `n / 2` opening brackets `'['` and `n / 2` closing brackets `']'`.

A string is called **balanced** if and only if:

-   It is the empty string, or
-   It can be written as `AB`, where both `A` and `B` are **balanced** strings, or
-   It can be written as `[C]`, where `C` is a **balanced** string.

You may swap the brackets at **any** two indices **any** number of times.

Return _the **minimum** number of swaps to make _`s` _**balanced**_.

**Example 1:**
```
Input: s = "][]["
Output: 1
Explanation: You can make the string balanced by swapping index 0 with index 3.
The resulting string is "[[]]".
```
**Example 2:**
```
Input: s = "]]][[["
Output: 2
Explanation: You can do the following to make the string balanced:
- Swap index 0 with index 4. s = "[]][][".
- Swap index 1 with index 5. s = "[[][]]".
The resulting string is "[[][]]".
```
**Example 3:**
```
Input: s = "[]"
Output: 0
Explanation: The string is already balanced.
```
**Constraints:**

-   `n == s.length`
-   `2 <= n <= 106`
-   `n` is even.
-   `s[i]` is either `'['` or `']'`.
-   The number of opening brackets `'['` equals `n / 2`, and the number of closing brackets `']'` equals `n / 2`.

---
```java
// Java 44ms(Beats 34.27%), Time O(N), Space O(N)
class Solution {
    public int minSwaps(String s) {
        Deque<Character> stack = new ArrayDeque<>();
        for (char c : s.toCharArray())
        {
            if (!stack.isEmpty() && c == ']' && stack.peek() == '[')
            {
                stack.pop();
                continue;
            }
            stack.push(c);
        }

        int k = stack.size() / 2;
        return k / 2 + k % 2;
    }
}
```

```java
// Java 14ms(Beats 76.66%), Time O(N), Space O(N)
class Solution {
    public int minSwaps(String s) {
        int count = 0;
        int max = 0;
        for (char c : s.toCharArray())
        {
            if (c == ']')
                count++;
            else
                count--;
            max = Math.max(max, count);
        }

        return (max + 1) / 2;
    }
}
```
---

將[]就近消除的話, 最後會得到`]]]][[[[`
因為`一次交換`最多可以處理`兩對[]`
若剩餘的對數為奇數, 則必須多一次交換來處理
因此答案為`(對數 + 1) / 2`


###### tags: `Leetcode` `Greedy` `Parenthesis`