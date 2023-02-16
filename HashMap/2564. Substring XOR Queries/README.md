# Leetcode - 2564. Substring XOR Queries (H-)

[Leetcode](https://leetcode.com/problems/substring-xor-queries/description/)

You are given a **binary string** `s`, and a **2D** integer array `queries` where `queries[i] = [firsti, secondi]`.

For the `ith` query, find the **shortest substring** of `s` whose **decimal value**, `val`, yields `secondi` when **bitwise XORed** with `firsti`. In other words, `val ^ firsti == secondi`.

The answer to the `ith` query is the endpoints (**0-indexed**) of the substring `[lefti, righti]` or `[-1, -1]` if no such substring exists. If there are multiple answers, choose the one with the **minimum** `lefti`.

_Return an array_ `ans` _where_ `ans[i] = [lefti, righti]` _is the answer to the_ `ith` _query._

A **substring** is a contiguous non-empty sequence of characters within a string.

**Example 1:**
```
Input: s = "101101", queries = [[0,5],[1,2]]
Output: [[0,2],[2,3]]
Explanation: For the first query the substring in range [0,2] is "101" 
which has a decimal value of 5, and 5 ^ 0 = 5, 
hence the answer to the first query is [0,2]. 
In the second query, the substring in range [2,3] is "11", 
and has a decimal value of 3, and 3 ^ 1 = 2. 
So, [2,3] is returned for the second query. 
```
**Example 2:**
```
Input: s = "0101", queries = [[12,8]]
Output: [[-1,-1]]
Explanation: In this example there is no substring that answers the query, 
hence [-1,-1] is returned.
```
**Example 3:**
```
Input: s = "1", queries = [[4,5]]
Output: [[0,0]]
Explanation: For this example, 
the substring in range [0,0] has a decimal value of 1, 
and 1 ^ 4 = 5. So, the answer is [0,0].
```
**Constraints:**

-   `1 <= s.length <= 104`
-   `s[i]` is either `'0'` or `'1'`.
-   `1 <= queries.length <= 105`
-   `0 <= firsti, secondi <= 109`

---
```java
// Java
class Solution {
    public int[][] substringXorQueries(String s, int[][] queries) {
        int[][] ret = new int[queries.length][2];
        HashMap<Integer, int[]> map = new HashMap<>();
        for (int i = 0; i < s.length(); i++) {
            if (s.charAt(i) == '0') {
                map.putIfAbsent(0, new int[] {i, i});
                continue;
            }

            int sum = 0;
            for (int j = i; j < s.length() && j < i + 32; j++) {
                sum = sum * 2 + (s.charAt(j) - '0');
                map.putIfAbsent(sum, new int[] {i, j});
            }
        }

        for (int i = 0; i < queries.length; i++) {
            int val = queries[i][0] ^ queries[i][1];
            ret[i] = map.getOrDefault(val, new int[] {-1, -1});
        }
        
        return ret;
    }
}
```
---

因為int最多32位數, 因此可將所有可能答案列出,
Time O(32N), N = s.length, 而非O(N^2)

###### tags: `Leetcode` `HashMap`
