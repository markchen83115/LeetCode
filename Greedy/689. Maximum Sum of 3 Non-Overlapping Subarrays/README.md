# Leetcode - 689. Maximum Sum of 3 Non-Overlapping Subarrays (M+)

[Leetcode](https://leetcode.com/problems/maximum-sum-of-3-non-overlapping-subarrays/)

Given an integer array `nums` and an integer `k`, find three non-overlapping subarrays of length `k` with maximum sum and return them.

Return the result as a list of indices representing the starting position of each interval (**0-indexed**). If there are multiple answers, return the lexicographically smallest one.

**Example 1:**

> **Input:** nums = [1,2,1,2,6,7,5,1], k = 2
> **Output:** [0,3,5]
> **Explanation:** Subarrays [1, 2], [2, 6], [7, 5] correspond to the starting indices [0, 3, 5].
> We could have also taken [2, 1], but an answer of [1, 3, 5] would be lexicographically larger.

**Example 2:**

> **Input:** nums = [1,2,1,2,1,2,1,2,1], k = 2
> **Output:** [0,2,4]

**Constraints:**

-   `1 <= nums.length <= 2 * 104`
-   `1 <= nums[i] < 216`
-   `1 <= k <= floor(nums.length / 3)`

---
```java
// Java 4ms(Beats 73.24%), Time O(N), Space O(N)
class Solution {
    public int[] maxSumOfThreeSubarrays(int[] nums, int k) {
        int n = nums.length;
        int[] arr = new int[n - k + 1];
        int sum = 0;
        for (int i = 0; i < n; i++)
        {
            sum += nums[i];
            if (i >= k)
                sum -= nums[i - k];
            if (i >= k - 1)
                arr[i - k + 1] = sum;
        }
        n = n - k + 1; // arr length
        // System.out.println(Arrays.toString(arr));
        
        int[] prefix = new int[n];
        int[] suffix = new int[n];
        suffix[n - 1] = n - 1;
        // prefix
        for (int i = 1; i < n; i++)
            if (arr[i] > arr[prefix[i - 1]])
                prefix[i] = i;
            else
                prefix[i] = prefix[i - 1];
        // suffix
        for (int i = n - 2; i >= 0; i--)
            if (arr[i] >= arr[suffix[i + 1]])
                suffix[i] = i;
            else
                suffix[i] = suffix[i + 1];

        // calculate
        int[] ret = new int[3];
        int currMax = 0;
        for (int i = k; i < n - k; i++)
        {
            int s = arr[prefix[i - k]] + arr[i] + arr[suffix[i + k]];
            if (currMax < s)
            {
                currMax = s;
                ret = new int[] {prefix[i - k], i, suffix[i + k]};
            }
        }

        return ret;
    }
}
```
---

題目要求三個subarray, 並且長度為k
將subarray先處理, 因此題目`nums=[1,2,1,2,6,7,5,1]`, 變為`arr=[3,3,3,8,13,12,6]`
即為`arr`內找三個數字, 但相隔距離至少為k

取三個數字, 可使用`three-pass`套路
提前處理prefix曾經出現的最大值, 以及suffix曾經出現的最大值


###### tags: `Leetcode` `Greedy` `Three-pass`