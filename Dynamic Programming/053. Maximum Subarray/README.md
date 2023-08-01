# Leetcode - 53. Maximum Subarray (E+)

[Leetcode](https://leetcode.com/problems/maximum-subarray/description/)

Given an integer array `nums`, find the  

subarray

 with the largest sum, and return _its sum_.

**Example 1:**
```
Input: nums = [-2,1,-3,4,-1,2,1,-5,4]
Output: 6
Explanation: The subarray [4,-1,2,1] has the largest sum 6.
```
**Example 2:**
```
Input: nums = [1]
Output: 1
Explanation: The subarray [1] has the largest sum 1.
```
**Example 3:**
```
Input: nums = [5,4,-1,7,8]
Output: 23
Explanation: The subarray [5,4,-1,7,8] has the largest sum 23.
```
**Constraints:**

-   `1 <= nums.length <= 105`
-   `-104 <= nums[i] <= 104`

**Follow up:** If you have figured out the `O(n)` solution, try coding another solution using the **divide and conquer** approach, which is more subtle.

---

```java
// Java DP 1ms Beats(100%), Time O(N), Space O(N)
class Solution {
    public int maxSubArray(int[] nums) {
        int[] dp = new int[nums.length];
        int max = nums[0];
        dp[0] = nums[0];
        for (int i = 1; i < nums.length; i++) {
            dp[i] = Math.max(dp[i - 1] + nums[i], nums[i]);
            max = Math.max(dp[i], max);
        }

        return max;
    }
}
```

```java
// Java 1ms Beats(100%), Time O(N), Space O(1)
class Solution {
    public int maxSubArray(int[] nums) {
        int max = nums[0], sum = 0;
        for (int i = 0; i < nums.length; i++) {
            sum += nums[i];
            max = Math.max(sum, max);
            if (sum < 0)
                sum = 0;
        }
        return max;
    }
}
```
---

```
x x x [x x i] x x x 
```

動態轉移方程式: `dp[i] = Max(dp[i - 1] + nums[i], nums[i])`



###### tags: `Leetcode` `Dynamic Programming` `Maximum Subarray`