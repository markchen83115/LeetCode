# Leetcode - 2401. Longest Nice Subarray (H-)

[Leetcode](https://leetcode.com/problems/longest-nice-subarray/)

You are given an array `nums` consisting of **positive** integers.

We call a subarray of `nums` **nice** if the bitwise **AND** of every pair of elements that are in **different** positions in the subarray is equal to `0`.

Return _the length of the **longest** nice subarray_.

A **subarray** is a **contiguous** part of an array.

**Note** that subarrays of length `1` are always considered nice.

**Example 1:**

> **Input:** nums = [1,3,8,48,10]
> **Output:** 3
> **Explanation:** The longest nice subarray is [3,8,48]. This subarray satisfies the conditions:
> - 3 AND 8 = 0.
> - 3 AND 48 = 0.
> - 8 AND 48 = 0.
> It can be proven that no longer nice subarray can be obtained, so we return 3.

**Example 2:**

> **Input:** nums = [3,1,5,11,13]
> **Output:** 1
> **Explanation:** The length of the longest nice subarray is 1. Any subarray of length 1 can be chosen.

**Constraints:**

-   `1 <= nums.length <= 105`
-   `1 <= nums[i] <= 109`

---
```java
// Java 3ms(Beats 98.64%), Time O(N), Space O(1)
class Solution {
    public int longestNiceSubarray(int[] nums) {
        int ret = 1;
        int n = nums.length;
        int count = 0;
        for (int i = 0, j = 0; i < n; i++)
        {
            while (j < n && (count & nums[j]) == 0)
            {
                count += nums[j];
                j++;
            }

            ret = Math.max(ret, j - i);

            count -= nums[i];
        }

        return ret;
    }
}
```
```java
// Java 43ms(Beats 10.35%), Time O(32N), Space(32)
class Solution {
    public int longestNiceSubarray(int[] nums) {
        int[] digit = new int[32];
        int n = nums.length;
        int ret = 1;
        for (int i = 0, j = 0; i < n; i++)  // j...i
        {
            int num = nums[i];
            for (int k = 0; k < 32; k++)
                digit[k] += ((num >> k) & 1);
            
            while (!isOK(digit))
            {
                num = nums[j];
                for (int k = 0; k < 32; k++)
                digit[k] -= ((num >> k) & 1);
                j++;
            }
            
            ret = Math.max(ret, i - j + 1);
        }
        return ret;
    }

    boolean isOK(int[] digit)
    {
        for (int i = 0; i < 32; i++)
            if (digit[i] > 1)
                return false;
        return true;
    }
}
```
---

參考[【每日一题】LeetCode 2401. Longest Nice Subarray - YouTube](https://www.youtube.com/watch?v=stXRx71prEE)

本題的本質就是找一段最長區間，使得這個區間內每個bit位上最多只出現一個1. 這其實就是和`003.Longest-Substring-Without-Repeating-Character`一樣的思想。

我們可以開一個長度為32的陣列來記錄每個bit位上出現1的頻次，但更精彩的做法就是用一個二進位數count本身來記錄。 count的每個bit可以記錄該位置出現了0次或1次的1。只要`(count & nums[j])==0`，那麼意味著沒有任何一個bit位上的1出現了超過1次，於是滑窗的右邊界就可以不斷右移。反之，就意味著不能繼續右移，那麼此時`[i,j-1]`就是以i開頭的、符合條件的最長區間。接下來我們只要移動左指針i，將nums[i]從count裡移出，那就可以繼續嘗試移動右指針了。


###### tags: `Leetcode` `Two Pointers` `Sliding window`