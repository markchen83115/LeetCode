# Leetcode - 2156. Find Substring With Given Hash Value (M)

[Leetcode](https://leetcode.com/problems/find-substring-with-given-hash-value/)

The hash of a **0-indexed** string `s` of length `k`, given integers `p` and `m`, is computed using the following function:

-   `hash(s, p, m) = (val(s[0]) * p0 + val(s[1]) * p1 + ... + val(s[k-1]) * pk-1) mod m`.

Where `val(s[i])` represents the index of `s[i]` in the alphabet from `val('a') = 1` to `val('z') = 26`.

You are given a string `s` and the integers `power`, `modulo`, `k`, and `hashValue.` Return `sub`,_ the **first** **substring** of _`s`_ of length _`k`_ such that _`hash(sub, power, modulo) == hashValue`.

The test cases will be generated such that an answer always **exists**.

A **substring** is a contiguous non-empty sequence of characters within a string.

**Example 1:**

> **Input:** s = "leetcode", power = 7, modulo = 20, k = 2, hashValue = 0
> **Output:** "ee"
> **Explanation:** The hash of "ee" can be computed to be hash("ee", 7, 20) = (5 * 1 + 5 * 7) mod 20 = 40 mod 20 = 0. 
> "ee" is the first substring of length 2 with hashValue 0. Hence, we return "ee".

**Example 2:**

> **Input:** s = "fbxzaad", power = 31, modulo = 100, k = 3, hashValue = 32
> **Output:** "fbx"
> **Explanation:** The hash of "fbx" can be computed to be hash("fbx", 31, 100) = (6 * 1 + 2 * 31 + 24 * 312) mod 100 = 23132 mod 100 = 32. 
> The hash of "bxz" can be computed to be hash("bxz", 31, 100) = (2 * 1 + 24 * 31 + 26 * 312) mod 100 = 25732 mod 100 = 32. 
> "fbx" is the first substring of length 3 with hashValue 32. Hence, we return "fbx".
> Note that "bxz" also has a hash of 32 but it appears later than "fbx".

**Constraints:**

-   `1 <= k <= s.length <= 2 * 104`
-   `1 <= power, modulo <= 109`
-   `0 <= hashValue < modulo`
-   `s` consists of lowercase English letters only.
-   The test cases are generated such that an answer always **exists**.

---
```java
// Java 9ms(Beats 92.11%), Time O(N), Space O(1)
class Solution {
    public String subStrHash(String s, int pow, int mod, int k, int hashValue) {
        long sum = 0;
        int n = s.length();
        int ret = 0;
        long qk = quickPow(pow, k - 1, mod);
        for (int i = n - 1; i >= 0; i--)
        {
            if (i + k < n)
            {
                sum = sum - (s.charAt(i + k) - 'a' + 1) * qk % mod;
                sum = (sum + mod) % mod;
            }

            sum = (sum * pow + (s.charAt(i) - 'a' + 1)) % mod;

            if (i + k <= n && sum == hashValue)
                ret = i;
        }

        return s.substring(ret, ret + k);
    }

    long quickPow(long x, long k, long mod) {
        if (k == 0) {
            return 1;
        }
        long y = quickPow(x, k / 2, mod) % mod;
        return k % 2 == 0 ? (y * y % mod) : (y * y % mod * x % mod);
    }
}
```
---

參考[【每日一题】LeetCode 2156. Find Substring With Given Hash Value - YouTube](https://youtu.be/bSeup58msps)

這是一門`Rolling Hash`的裸題，就是將一個字串轉換為`26進位`的整數。注意到`Hash`的高位在右邊，所以我們需要從右往左進行滑窗滾定。

每個回合裡，先砍掉原先的最高位乘以`26^(k-1)`，然後整體乘上`26`，再加上新引入的最低位。

由於牽涉到減法，可能會產生負數，所以每次取模時採用`(x+M)%M`的方式較為穩健。


###### tags: `Leetcode` `String` `Rolling Hash`