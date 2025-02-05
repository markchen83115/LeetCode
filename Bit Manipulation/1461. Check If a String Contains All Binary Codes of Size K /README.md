# Leetcode - 1461. Check If a String Contains All Binary Codes of Size K (M+)

[Leetcode](https://leetcode.com/problems/check-if-a-string-contains-all-binary-codes-of-size-k/)

Given a binary string `s` and an integer `k`, return `true` _if every binary code of length_ `k` _is a substring of_ `s`. Otherwise, return `false`.

**Example 1:**

> **Input:** s = "00110110", k = 2
> **Output:** true
> **Explanation:** The binary codes of length 2 are "00", "01", "10" and "11". They can be all found as substrings at indices 0, 1, 3 and 2 respectively.

**Example 2:**

> **Input:** s = "0110", k = 1
> **Output:** true
> **Explanation:** The binary codes of length 1 are "0" and "1", it is clear that both exist as a substring. 

**Example 3:**

> **Input:** s = "0110", k = 2
> **Output:** false
> **Explanation:** The binary code "00" is of length 2 and does not exist in the array.

**Constraints:**

-   `1 <= s.length <= 5 * 105`
-   `s[i]` is either `'0'` or `'1'`.
-   `1 <= k <= 20`

---
```java
// Java Bit Manipulation 6ms(Beats 100.00%), Time O(N), Space O(k^2)
class Solution {
    public boolean hasAllCodes(String s, int k) {
        int n = s.length();
        if (n <= k)
            return false;
        int count = 1 << k;
        int num = k > 1 ? Integer.parseInt(s.substring(n - k + 1), 2) << 1 : 0;
        boolean[] seen = new boolean[count];
        for (int i = n - k; i >= 0; i--)
        {
            num = (num >> 1) + ((s.charAt(i) - '0') << k - 1);
            if (!seen[num])
            {
                seen[num] = true;
                count--;
            }
            if (count == 0)
                return true;
            if (i < count)
                return false;
        }

        return false;
    }
}
```
```java
// Java Sliding Window 165ms(Beats 48.65%), TIme O(N * k), Space O(k^2)
class Solution {
    public boolean hasAllCodes(String s, int k) {
        int n = s.length();
        HashSet<String> set = new HashSet<>();
        for (int i = 0; i <= n - k; i++)
            set.add(s.substring(i, i + k));
        
        return quickPow(2, k) == set.size();
    }

    int quickPow(int a, int k)
    {
        if (k == 0)
            return 1;
        if (k == 1)
            return a;
        if (k % 2 == 1)
            return a * quickPow(a * a, k / 2);
        return quickPow(a * a, k / 2);
    }
}
```
---

利用`bit`計算, `k = 2`
```
00110110
     ___(a)
    ___(b)
``` 
將`(a)` `110` 左移`1`之後, 變成`11`
此時再加上`s.charAt(i) << k - 1`則變為`(b)`
故可達到`O(N)`的效果


###### tags: `Leetcode` `Bit Manipulation`