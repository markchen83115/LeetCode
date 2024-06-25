# Leetcode - 995. Minimum Number of K Consecutive Bit Flips (H-)

[Leetcode](https://leetcode.com/problems/minimum-number-of-k-consecutive-bit-flips/)

You are given a binary array `nums` and an integer `k`.

A **k-bit flip** is choosing a **subarray** of length `k` from `nums` and simultaneously changing every `0` in the subarray to `1`, and every `1` in the subarray to `0`.

Return _the minimum number of **k-bit flips** required so that there is no _`0`_ in the array_. If it is not possible, return `-1`.

A **subarray** is a **contiguous** part of an array.

**Example 1:**
```
Input: nums = [0,1,0], k = 1
Output: 2
Explanation: Flip nums[0], then flip nums[2].
```
**Example 2:**
```
Input: nums = [1,1,0], k = 2
Output: -1
Explanation: No matter how we flip subarrays of size 2, we cannot make the array become [1,1,1].
```
**Example 3:**
```
Input: nums = [0,0,0,1,0,1,1,0], k = 3
Output: 3
Explanation: 
Flip nums[0],nums[1],nums[2]: nums becomes [1,1,1,1,0,1,1,0]
Flip nums[4],nums[5],nums[6]: nums becomes [1,1,1,1,1,0,0,0]
Flip nums[5],nums[6],nums[7]: nums becomes [1,1,1,1,1,1,1,1]
```
**Constraints:**

-   `1 <= nums.length <= 105`
-   `1 <= k <= nums.length`

---
```java
// Java 5ms (Beats 88.13%), Time O(N), Space O(N)
class Solution {
    public int minKBitFlips(int[] nums, int k) {
        int n = nums.length;
        int[] diff = new int[n+1];
        int curr = 0;
        int ret = 0;

        for (int i = 0; i <= n - k; i++)
        {
            curr += diff[i];
            int flip = 1 - ((nums[i] + curr) % 2);
            // nums[i] = (nums[i] + curr + flip) % 2;
            ret += flip;
            curr += flip;
            diff[i+k] = -flip;
            // System.out.println(nums[i]);
        }

        for (int i = n-k+1; i < n; i++)
        {
            curr += diff[i];
            // System.out.println((nums[i] + curr) % 2);
            if ((nums[i] + curr) % 2 != 1)
                return -1;
        }
		
        return ret;
    }
}
```


---

###### tags: `Leetcode` `Others` `掃描線/差分陣列`