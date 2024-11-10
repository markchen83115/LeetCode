# Leetcode - 3097. Shortest Subarray With OR at Least K II (M)

[Leetcode](https://leetcode.com/problems/shortest-subarray-with-or-at-least-k-ii/)

You are given an array `nums` of **non-negative** integers and an integer `k`.

An array is called **special** if the bitwise `OR` of all of its elements is **at least** `k`.

Return _the length of the **shortest** **special** **non-empty**_ _subarray of_ `nums`, _or return_ `-1` _if no special subarray exists_.

**Example 1:**

> **Input:** nums = [1,2,3], k = 2
> 
> **Output:** 1
> 
> **Explanation:**
> 
> The subarray `[3]` has `OR` value of `3`. Hence, we return `1`.

**Example 2:**

> **Input:** nums = [2,1,8], k = 10
> 
> **Output:** 3
> 
> **Explanation:**
> 
> The subarray `[2,1,8]` has `OR` value of `11`. Hence, we return `3`.

**Example 3:**

> **Input:** nums = [1,2], k = 0
> 
> **Output:** 1
> 
> **Explanation:**
> 
> The subarray `[1]` has `OR` value of `1`. Hence, we return `1`.

**Constraints:**

-   `1 <= nums.length <= 2 * 105`
-   `0 <= nums[i] <= 109`
-   `0 <= k <= 109`

---
```java
// Java 59ms(Beats 37.78%), Time O(32N), Space O(32)
class Solution {
    int[] bitArr = new int[32];
    public int minimumSubarrayLength(int[] nums, int k) {
        if (k == 0)
            return 1;
        int j = 0;
        int n = nums.length;
        int ret = n + 1;
        for (int i = 0; i < n; i++)
        {
            while (j < n && bitVal() < k)
            {
                add(nums[j]);
                j++;
            }
            if (bitVal() >= k)
                ret = Math.min(ret, j - i);

            remove(nums[i]);
        }

        return ret < n + 1 ? ret : -1;
    }

    void add(int k) // insert element from window, time: O(32)
    {
        for (int i = 0; i < 32; i++)
        {
            bitArr[i] += k & 1;
            k >>= 1;
        }
    }

    void remove(int k)  // remove element from window, time: O(32)
    {
        for (int i = 0; i < 32; i++)
        {
            bitArr[i] -= k & 1;
            k >>= 1;
        }
    }

    int bitVal()    // convert 32-size bits array to integer, time: O(32)
    {
        int ret = 0;
        for (int i = 0, a = 1; i < 32; i++)
        {
            if (bitArr[i] > 0)
                ret += a;
            a <<= 1;
        }
        return ret;
    }
}
```
---

因為數字會越OR越大
因此使用sliding window, 右端點向右移動會越大, 左端點向右移動則會越小

滑動窗口 = 一個bits array (int[32])
右端點向右移動時, 將bits array加入右端點的bit位
左端點向右移動時, 將bits array扣除左端點的bit位
檢查目前窗口是否>=k, 將bits array > 0的bit位做加總

###### tags: `Leetcode` `Two Pointers` `Sliding window`