# Leetcode - 2516. Take K of Each Character From Left and Right (M+)

[Leetcode](https://leetcode.com/problems/take-k-of-each-character-from-left-and-right/)

You are given a string `s` consisting of the characters `'a'`, `'b'`, and `'c'` and a non-negative integer `k`. Each minute, you may take either the **leftmost** character of `s`, or the **rightmost** character of `s`.

Return_ the **minimum** number of minutes needed for you to take **at least** _`k`_ of each character, or return _`-1`_ if it is not possible to take _`k`_ of each character._

**Example 1:**

> **Input:** s = "aabaaaacaabc", k = 2
> **Output:** 8
> **Explanation:** 
> Take three characters from the left of s. You now have two 'a' characters, and one 'b' character.
> Take five characters from the right of s. You now have four 'a' characters, two 'b' characters, and two 'c' characters.
> A total of 3 + 5 = 8 minutes is needed.
> It can be proven that 8 is the minimum number of minutes needed.

**Example 2:**

> **Input:** s = "a", k = 1
> **Output:** -1
> **Explanation:** It is not possible to take one 'b' or 'c' so return -1.

**Constraints:**

-   `1 <= s.length <= 105`
-   `s` consists of only the letters `'a'`, `'b'`, and `'c'`.
-   `0 <= k <= s.length`

---
```java
// Java 19ms(Beats 16.03%), Time O(N), Space O(N)
class Solution {
    public int takeCharacters(String s, int k) {
        int n = s.length();
        int A = 0, B = 0, C = 0;
        for (int i = 0; i < n; i++)
        {
            if (s.charAt(i) == 'a') A++;
            else if (s.charAt(i) == 'b') B++;
            else C++;

        }
        
        if (A < k || B < k || C < k)
            return -1;
        
        A -= k;
        B -= k;
        C -= k;
        
        int count = 0;
        int a = 0, b = 0, c = 0;
        int j = 0;
        for (int i = 0; i < n; i++)
        {
            while (a <= A && b <= B && c <= C)
            {
                count = Math.max(count, j - i);
                if (j == n)
                    break;
                if (s.charAt(j) == 'a') a++;
                else if (s.charAt(j) == 'b') b++;
                else c++;
                j++;
            }

            if (s.charAt(i) == 'a') a--;
            else if (s.charAt(i) == 'b') b--;
            else c--;
        }

        return n - count;
    }
}

// Java 13ms(Beats 74.55%), Time O(N), Space O(N)
class Solution {
    public int takeCharacters(String s, int k) {
        if (k == 0)
            return 0;
        String t = s + s;
        int n1 = s.length();
        int n2 = t.length();
        int need = 3;
        int[] freq = new int[3];
        int ret = n2;
        int j = 0;
		// 以nums[0]為頭 從前往後
        for (int i = 0; i < n1; i++)
        {
            if (++freq[t.charAt(i) - 'a'] == k)
                need--;
            if (need == 0)
            {
                ret = Math.min(ret, i + 1);
                break;
            }
        }

        freq = new int[3];
        need = 3;
		// 以nums[n - 1]為頭 從後往前
        for (int i = n1 - 1; i >= 0; i--)
        {
            if (++freq[t.charAt(i) - 'a'] == k)
                need--;
            if (need == 0)
            {
                ret = Math.min(ret, n1 - i);
                break;
            }
        }

        freq = new int[3];
        need = 3;
		// 頭尾都取
        for (int i = 0; i < n1; i++)
        {
            while (j < n2 && need > 0)
            {
                if (++freq[t.charAt(j) - 'a'] == k)
                    need--;
                j++;
            }

            if (need == 0 && j >= n1)
                ret = Math.min(ret, j - i);
            
            if (--freq[t.charAt(i) - 'a'] == k - 1)
                need++;
        }

        return ret <= n1 ? ret : -1;
    }
}
```
---

先計算ABC的個數, 若其中一個不足k則代表無解

因為`右端點j`越往右邊移動, 代表能取得的ABC個數越少
`左端點i`越往右邊移動, 代表取得的ABC個數越多
因此利用滑動窗口在中間尋找一個最大長度, 使得ABC的個數不得超過各自的需求值
答案為`長度 - 窗口最大長度`

###### tags: `Leetcode` `Two Pointers` `Sliding window`