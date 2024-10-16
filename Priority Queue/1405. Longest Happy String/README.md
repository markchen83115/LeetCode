# Leetcode - 1405. Longest Happy String (H-)

[Leetcode](https://leetcode.com/problems/longest-happy-string/)

A string `s` is called **happy** if it satisfies the following conditions:

-   `s` only contains the letters `'a'`, `'b'`, and `'c'`.
-   `s` does not contain any of `"aaa"`, `"bbb"`, or `"ccc"` as a substring.
-   `s` contains **at most** `a` occurrences of the letter `'a'`.
-   `s` contains **at most** `b` occurrences of the letter `'b'`.
-   `s` contains **at most** `c` occurrences of the letter `'c'`.

Given three integers `a`, `b`, and `c`, return _the **longest possible happy **string_. If there are multiple longest happy strings, return _any of them_. If there is no such string, return _the empty string _`""`.

A **substring** is a contiguous sequence of characters within a string.

**Example 1:**
```
Input: a = 1, b = 1, c = 7
Output: "ccaccbcc"
Explanation: "ccbccacc" would also be a correct answer.
```
**Example 2:**
```
Input: a = 7, b = 1, c = 0
Output: "aabaa"
Explanation: It is the only correct answer in this case.
```
**Constraints:**

-   `0 <= a, b, c <= 100`
-   `a + b + c > 0`

---
```java
// Java 1ms(Beats 77.96%), Time O(NlogN), Space O(N)
class Solution {
    public String longestDiverseString(int a, int b, int c) {
                StringBuilder sb = new StringBuilder();
        PriorityQueue<int[]> pq = new PriorityQueue<>((x, y) -> y[0] - x[0]);
        if (a > 0)
            pq.offer(new int[]{a, 0});
        if (b > 0)
            pq.offer(new int[]{b, 1});
        if (c > 0)
            pq.offer(new int[]{c, 2});

        while (!pq.isEmpty()) 
        {
            int[] cur = pq.poll();
            char ch =  (char) ('a' + cur[1]);
            if (sb.length() >= 2 && sb.charAt(sb.length() - 1) == ch && sb.charAt(sb.length() - 2) == ch)
            {
                if (pq.isEmpty())
                    return sb.toString();

                int[] cur2 = pq.poll();
                sb.append((char) ('a' + cur2[1]));
                if (--cur2[0] > 0)
                    pq.offer(new int[] {cur2[0], cur2[1]});
            }

            sb.append(ch);
            if (--cur[0] > 0)
                pq.offer(new int[]{cur[0], cur[1]});
        }

        return sb.toString();
    }
}
```
---

每次都取出數量最多的字母, 並檢查是否已經出現過兩次
若已出現兩次, 則在取出次多的字母
若無法取出次多的字母, 代表已達到最大長度並return


###### tags: `Leetcode` `Priority Queue` `Arrangement with Stride`
