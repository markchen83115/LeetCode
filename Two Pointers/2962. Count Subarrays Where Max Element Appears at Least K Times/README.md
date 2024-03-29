# Leetcode - 2962. Count Subarrays Where Max Element Appears at Least K Times (M)

[Leetcode](https://leetcode.com/problems/count-subarrays-where-max-element-appears-at-least-k-times/)

You are given an integer array `nums` and a **positive** integer `k`.

Return _the number of subarrays where the **maximum** element of _`nums`_ appears **at least** _`k`_ times in that subarray._

A **subarray** is a contiguous sequence of elements within an array.

**Example 1:**
```
Input: nums = [1,3,2,3,3], k = 2
Output: 6
Explanation: The subarrays that contain the element 3 at least 2 times are: [1,3,2,3], [1,3,2,3,3], [3,2,3], [3,2,3,3], [2,3,3] and [3,3].
```
**Example 2:**
```
Input: nums = [1,4,2,1], k = 3
Output: 0
Explanation: No subarray contains the element 4 at least 3 times.
```
**Constraints:**

-   `1 <= nums.length <= 105`
-   `1 <= nums[i] <= 106`
-   `1 <= k <= 105`

---
```java
// Java 4ms (Beats 100.00%), Time O(N), Space O(1)
class Solution {
    public long countSubarrays(int[] nums, int k) {
        HashMap<Integer, Integer> indexMap = new HashMap<>();
        int max = 0;
        for (int num : nums)
            max = Math.max(max, num);

        int count = 0;
        int j = 0;
        long ret = 0;
        for (int i = 0; i < nums.length; i++)
        {
            if (nums[i] == max)
                count++;
            while (count >= k)
            {
                if (nums[j] == max)
                    count--;
                j++;
            }

            ret += j;
        }

        return ret;
    }
}

```
---

[refer link](https://leetcode.com/problems/count-subarrays-where-max-element-appears-at-least-k-times/solutions/4940242/simplified-algorithm-explained-with-visual-example-c-java)

```
Input: nums = [1,3,2,3,3], k = 2
Output: 6

```

![Screenshot (3412).png](https://assets.leetcode.com/users/images/9721935e-908e-4d70-a889-742e96756bb0_1711683294.03512.png)

![Screenshot (3413).png](https://assets.leetcode.com/users/images/f82d426d-1b7c-4e9e-ba3b-0c1fbbfcd5a4_1711683323.9971247.png)

![Screenshot (3414).png](https://assets.leetcode.com/users/images/f4c99bf1-c68d-44f1-91a3-1be7fcc020e5_1711683336.0002992.png)

![Screenshot (3415).png](https://assets.leetcode.com/users/images/540f12a6-7383-4cae-b21c-23259ceacffa_1711683346.4058976.png)

![Screenshot (3416).png](https://assets.leetcode.com/users/images/a85b4efb-b78d-46fc-931d-06a2e7d9cc61_1711683373.2839067.png)

![Screenshot (3417).png](https://assets.leetcode.com/users/images/6b8ef8db-5648-4430-9003-4b0b5db1f0c9_1711683423.6166074.png)

![Screenshot (3418).png](https://assets.leetcode.com/users/images/ec7ef1ea-c9b1-4d00-a55c-c1573df53197_1711683450.1824498.png)

![Screenshot (3419).png](https://assets.leetcode.com/users/images/d270303a-418e-409a-bcac-41826e12aba1_1711683479.4220455.png)

![Screenshot (3421).png](https://assets.leetcode.com/users/images/495dcdaa-36de-4ffc-bff9-89d2cdc6a552_1711683504.0886233.png)

![Screenshot (3422).png](https://assets.leetcode.com/users/images/fbc91b3c-d408-4c66-8276-e97e8d4c6661_1711683524.8151224.png)


###### tags: `Leetcode` `Two Pointers` `Sliding window`