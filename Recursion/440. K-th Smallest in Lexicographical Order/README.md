# Leetcode - 440. K-th Smallest in Lexicographical Order (H-)

[Leetcode](https://leetcode.com/problems/k-th-smallest-in-lexicographical-order/)

Given two integers `n` and `k`, return _the_ `kth` _lexicographically smallest integer in the range_ `[1, n]`.

**Example 1:**

**Input:** n = 13, k = 2
**Output:** 10
**Explanation:** The lexicographical order is \[1, 10, 11, 12, 13, 2, 3, 4, 5, 6, 7, 8, 9\], so the second smallest number is 10.

**Example 2:**

**Input:** n = 1, k = 1
**Output:** 1

**Constraints:**

-   `1 <= k <= n <= 109`

---
```java
// Java 0ms(Beats 100.00%), Time O(Log(N)+K), Space O(1)
class Solution {
    public int findKthNumber(int n, int k) {
        int curr = 1;
        k = k - 1;
        while (k > 0)
        {
            int step = calStep(n, curr);
            if (step <= k)
            {
                k -= step;
                curr += 1;
            }
            else
            {
                curr *= 10;
                k -= 1;
            }
        }

        return curr;
    }

    int calStep (int n, int prefix)
    {
        long exp = 1;
        int count = 0;
        long lower = prefix;
        long upper = prefix;
        while (lower <= n)
        {
            count += Math.min(upper, n) - lower + 1;
            exp *= 10;
            lower = prefix * exp;
            upper = prefix * exp + (exp - 1);
        }
        return count;
    }
}
/*
            1               2           3
        10...19.        20...29.   30...39
     100..109.  190...199
*/
```

---
參考[Concise/Easy-to-understand Java 5ms solution with Explaination
](https://leetcode.com/problems/k-th-smallest-in-lexicographical-order/solutions/92242/concise-easy-to-understand-java-5ms-solution-with-explaination)
![image](https://hackmd.io/_uploads/ByPexqTpA.png)

以`i=1`作為起點, 計算抵達`i+1`的步數為`step`
若`step <= k`代表可以跳到`i+1`並且`k = k - step`
若`step < k`代表目標一定在`i+1`之前
接續計算以`i*10 (1 -> 10)`為起點到達`i+1 (11)`的步數...以此類推


###### tags: `Leetcode` `Recursion` `Digit counting & finding`