# Leetcode - 1248. Count Number of Nice Subarrays (M+)

[Leetcode](https://leetcode.com/problems/count-number-of-nice-subarrays)

Given an array of integers `nums` and an integer `k`. A continuous subarray is called **nice** if there are `k` odd numbers on it.

Return _the number of **nice** sub-arrays_.

**Example 1:**
```
Input: nums = [1,1,2,1,1], k = 3
Output: 2
Explanation: The only sub-arrays with 3 odd numbers are [1,1,2,1] and [1,2,1,1].
```
**Example 2:**
```
Input: nums = [2,4,6], k = 1
Output: 0
Explanation: There are no odd numbers in the array.
```
**Example 3:**
```
Input: nums = [2,2,2,1,2,2,1,2,2,2], k = 2
Output: 16
```
**Constraints:**

-   `1 <= nums.length <= 50000`
-   `1 <= nums[i] <= 10^5`
-   `1 <= k <= nums.length`

---
```java
// Java 8ms (Beats 87.08%), Time O(N), Space O(1)
class Solution {
    public int numberOfSubarrays(int[] nums, int k) {
        return atMost(nums, k) - atMost(nums, k - 1);
    }

    public int atMost(int[] nums, int k)
    {
        int j = 0;
        int ret = 0;
        for (int i = 0; i < nums.length; i++)
        {
            k -= nums[i] & 1;
            while (k < 0)
            {
                k += nums[j++] & 1;
            }
            ret += i - j + 1;
        }
        return ret;
    }
}
```
```java
// Java 5ms (Beats 98.23%), Time O(N), Space O(1)
class Solution {
    public int numberOfSubarrays(int[] nums, int k) {
        int ret = 0;
        int j = 0;
        int count = 0;
        for (int i = 0; i < nums.length; i++)
        {
            if ((nums[i] & 1) == 1)
            {
                k--;
                count = 0;
            }
            while (k == 0)
            {
                k += nums[j++] & 1;
                count++;
            }
            ret += count;
        }

        return ret;

    }
}
```
---
**Solution 1: atMost**
======================

Have you read this? [992\. Subarrays with K Different Integers](https://leetcode.com/problems/subarrays-with-k-different-integers/discuss/523136/JavaC%2B%2BPython-Sliding-Window)  
Exactly `K` times = at most `K` times - at most `K - 1` times


Solution 2: One pass
=====================

Actually it's same as three pointers.  
Though we use `count` to count the number of even numebers.  
Insprired by @yannlecun.


###### tags: `Leetcode` `Two Pointers` `Sliding window`