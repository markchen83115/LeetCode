# Leetcode - 1043. Partition Array for Maximum Sum (M+)

[Leetcode](https://leetcode.com/problems/partition-array-for-maximum-sum/)

Given an integer array `arr`, partition the array into (contiguous) subarrays of length **at most** `k`. After partitioning, each subarray has their values changed to become the maximum value of that subarray.

Return _the largest sum of the given array after partitioning. Test cases are generated so that the answer fits in a **32-bit** integer._

**Example 1:**
```
Input: arr = [1,15,7,9,2,5,10], k = 3
Output: 84
Explanation: arr becomes [15,15,15,9,10,10,10]
```
**Example 2:**
```
Input: arr = [1,4,1,5,7,3,6,1,9,9,3], k = 4
Output: 83
```
**Example 3:**
```
Input: arr = [1], k = 1
Output: 1
```
**Constraints:**

-   `1 <= arr.length <= 500`
-   `0 <= arr[i] <= 109`
-   `1 <= k <= arr.length`

---
```java
// Java 7ms (Beats 58.48%), Time O(NK), Space O(N)
class Solution {
    public int maxSumAfterPartitioning(int[] arr, int k) {
        int n = arr.length;
        int[] dp = new int[n+1];
        dp[0] = 0;
        for (int i = 0; i < n; i++)
        {
            int max = 0;
            for (int j = i; j >= i-k+1 && j >= 0; j--)
            {
                max = Math.max(max, arr[j]);
                dp[i+1] = Math.max(dp[i+1], dp[j] + max * (i-j+1));
            }
            // System.out.println(i + ":" + dp[i]);
        }

        return dp[n];
    }

    // dp[i] = largest sum of the arr[0:i] after partitioning
    // [x x x x x] [j x i]
    // dp[i] = max (dp[j-1] + MAX * (i-j+1)) for j = i, i-1, ... i-k+1
}
```
---
```
[x x x x x] [j x i]
```
令`dp[i] = largest sum of the arr[0:i] after partitioning`
`dp[i] = max (dp[j-1] + MAX * (i-j+1)) for j = i, i-1, ... i-k+1`



###### tags: `Leetcode` `Dynamic Programming` `基本型II`