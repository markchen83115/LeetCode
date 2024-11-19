# Leetcode - 2461. Maximum Sum of Distinct Subarrays With Length K (M)

[Leetcode](https://leetcode.com/problems/maximum-sum-of-distinct-subarrays-with-length-k/)

You are given an integer array `nums` and an integer `k`. Find the maximum subarray sum of all the subarrays of `nums` that meet the following conditions:

-   The length of the subarray is `k`, and
-   All the elements of the subarray are **distinct**.

Return _the maximum subarray sum of all the subarrays that meet the conditions._ If no subarray meets the conditions, return `0`.

_A **subarray** is a contiguous non-empty sequence of elements within an array._

**Example 1:**

> **Input:** nums = [1,5,4,2,9,9,9], k = 3
> **Output:** 15
> **Explanation:** The subarrays of nums with length 3 are:
> - [1,5,4] which meets the requirements and has a sum of 10.
> - [5,4,2] which meets the requirements and has a sum of 11.
> - [4,2,9] which meets the requirements and has a sum of 15.
> - [2,9,9] which does not meet the requirements because the element 9 is repeated.
> - [9,9,9] which does not meet the requirements because the element 9 is repeated.
> We return 15 because it is the maximum subarray sum of all the subarrays that meet the conditions

**Example 2:**

> **Input:** nums = [4,4,4], k = 3
> **Output:** 0
> **Explanation:** The subarrays of nums with length 3 are:
> - [4,4,4] which does not meet the requirements because the element 4 is repeated.
> We return 0 because no subarrays meet the conditions.

**Constraints:**

-   `1 <= k <= nums.length <= 105`
-   `1 <= nums[i] <= 105`

---
```java
// Java 6ms(Beats 98.76%), Time O(N), Space O(N)
class Solution {
    public long maximumSubarraySum(int[] nums, int k) {
        long ret = 0;
        long sum = 0;
        int j = 0;
        int n = nums.length;
        int[] freq = new int[100005];
        int duplicates = 0;
        for (int i = 0; i < n; i++)
        {
            while (j < n && j - i < k)
            {
                if (++freq[nums[j]] == 2)
                    duplicates++;
                sum += nums[j];
                j++;
            }

            if (duplicates == 0 && j - i == k)
                ret = Math.max(ret, sum);
            
            if (--freq[nums[i]] == 1)
                duplicates--;
            sum -= nums[i];
        }

        return ret;
    }
}
```
```java
// Java 8ms(Beats 97.26%), Time O(N), Space O(N)
class Solution {
    public long maximumSubarraySum(int[] nums, int k) {
        long ret = 0;
        long sum = 0;
        int j = 0;
        int n = nums.length;
        int[] freq = new int[100005];
        int count = 0;
        for (int i = 0; i < n; i++)
        {
            if (++freq[nums[i]] == 1)
                count++;
            sum += nums[i];

            if (count == k)
                ret = Math.max(ret, sum);

            if (i >= k - 1)
            {
                if (--freq[nums[i - k + 1]] == 0)
                    count--;
                sum -= nums[i - k + 1];
            }
        }

        return ret;
    }
}
```
---

記錄每個number出現的次數, 並用count來計算目前有多少個distinct數字
1.  新加入一個新數字時

```java
freq[num]++;
if (freq[num] == 1) 
  count++;
```

2.  当移出一个数字时

```java
freq[num]--;
if (freq[num] == 0) 
  count--;
```


###### tags: `Leetcode` `Two Pointers` `Sliding window : Distinct Characters`