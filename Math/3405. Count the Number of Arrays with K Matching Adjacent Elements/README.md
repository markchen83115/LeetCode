# Leetcode - 3405. Count the Number of Arrays with K Matching Adjacent Elements (H-)

[Leetcode](https://leetcode.com/problems/count-the-number-of-arrays-with-k-matching-adjacent-elements/)

You are given three integers `n`, `m`, `k`. A **good array** `arr` of size `n` is defined as follows:

-   Each element in `arr` is in the **inclusive** range `[1, m]`.
-   _Exactly_ `k` indices `i` (where `1 <= i < n`) satisfy the condition `arr[i - 1] == arr[i]`.

Return the number of **good arrays** that can be formed.

Since the answer may be very large, return it **modulo **`109 + 7`.

**Example 1:**

> **Input:** n = 3, m = 2, k = 1
> 
> **Output:** 4
> 
> **Explanation:**
> 
> -   There are 4 good arrays. They are `[1, 1, 2]`, `[1, 2, 2]`, `[2, 1, 1]` and `[2, 2, 1]`.
> -   Hence, the answer is 4.

**Example 2:**

> **Input:** n = 4, m = 2, k = 2
> 
> **Output:** 6
> 
> **Explanation:**
> 
> -   The good arrays are `[1, 1, 1, 2]`, `[1, 1, 2, 2]`, `[1, 2, 2, 2]`, `[2, 1, 1, 1]`, `[2, 2, 1, 1]` and `[2, 2, 2, 1]`.
> -   Hence, the answer is 6.

**Example 3:**

> **Input:** n = 5, m = 2, k = 0
> 
> **Output:** 2
> 
> **Explanation:**
> 
> -   The good arrays are `[1, 2, 1, 2, 1]` and `[2, 1, 2, 1, 2]`. Hence, the answer is 2.

**Constraints:**

-   `1 <= n <= 105`
-   `1 <= m <= 105`
-   `0 <= k <= n - 1`

---
```java
// Java 27ms(Beats 78.43%), Time O(N), Space O(1)
class Solution {
    int mod = (int)1e9 + 7;
    public int countGoodArrays(int n, int m, int k) {
        // m * (m-1) * (m-1) * ... * (m-1) * C(n取k)
        //                   n-k-1個

        return (int) (m * quickPow(m-1, n-k-1) % mod * comb(n-1, k) % mod);
    }

    long quickPow(long base, int exp) {
        long result = 1;
        while (exp > 0) {
            if ((exp & 1) == 1) {
                result = (result * base) % mod;
            }
            base = (base * base) % mod;
            exp >>= 1;
        }
        return result;
    }

    long comb(int n, int k) {
        if (k > n) return 0;
        long numerator = 1;
        long denominator = 1;

        for (int i = 0; i < k; i++) {
            numerator = (numerator * (n - i)) % mod;
            denominator = (denominator * (i + 1)) % mod;
        }

        return (numerator * inverse(denominator)) % mod;
    }

    long inverse(long a) {
        return quickPow(a, mod - 2);
    }
}
```
---

參考[【每日一题】LeetCode 3405. Count the Number of Arrays with K Matching Adjacent Elements - YouTube](https://youtu.be/V2AN0SKinW0)

考慮到剛好有k個位置的元素與其左邊元素相同，那麼將其與其左邊元素「合併」後
數組裡可看做只有n-k個元素，並且這些元素在相鄰的位置上不重複

從原數組挑出k個位置，但第一個位置不能選, 因為他沒有左邊元素
因此總共有`comb(n-1, k)`種方案

任何上述的方案，對於「合併」後的數組的`n-k`個元素，要求相鄰之間不重複，有多少種方案？第一個位置有`m`種選擇，之後每個位置都只有`m-1`種選擇。故總共有`m * m^(n-k-1)`種方案

所以最終答案就是簡單的數學表達式 `comb(n-1, k) * m * m^(n-k-1)`

因為`n`和`k`是`10^5`，所以不能用`O(n*k)`的複雜度計算組合數任何`n`以內的組合數
我們可以直接硬算`comb(n-1, k) = (n-1)!/k!/(n-1-k)!`, 其中階乘的複雜度就是`O(n)`
但是涉及到了除法，需要介入逆元
逆元的計算公式是`inv_x = quickPow(x, (MOD-2))`



###### tags: `Leetcode` `Math` `Combinatorics`