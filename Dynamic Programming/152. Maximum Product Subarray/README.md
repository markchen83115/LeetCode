# Leetcode - 152. Maximum Product Subarray (M+)

[Leetcode](https://leetcode.com/problems/maximum-product-subarray/description/)

Given an integer array `nums`, find a subarray that has the largest product, and return _the product_.

The test cases are generated so that the answer will fit in a **32-bit** integer.

**Example 1:**
```
Input: nums = [2,3,-2,4]
Output: 6
Explanation: [2,3] has the largest product 6.
```
**Example 2:**
```
Input: nums = [-2,0,-1]
Output: 0
Explanation: The result cannot be 2, because [-2,-1] is not a subarray.
```
**Constraints:**

-   `1 <= nums.length <= 2 * 104`
-   `-10 <= nums[i] <= 10`
-   The product of any prefix or suffix of `nums` is **guaranteed** to fit in a **32-bit** integer.

---
```java
// Java DP 4ms(Beats 14.16%), Time O(N), Space O(N)
class Solution {
    public int maxProduct(int[] nums) {
        int n = nums.length;
        int[][] dp = new int[n][2];
        int ret = nums[0];
        dp[0][0] = nums[0];
        dp[0][1] = nums[0];

        for (int i = 1; i < n; i++) {
            // Max product of subarray which end at i
            dp[i][0] = Math.max( Math.max(dp[i-1][0] * nums[i], dp[i-1][1] * nums[i]), nums[i]);
            // Min product of subarray which end at i
            dp[i][1] = Math.min( Math.min(dp[i-1][0] * nums[i], dp[i-1][1] * nums[i]), nums[i]);

            ret = Math.max( Math.max(ret, dp[i][0]), dp[i][1]);
        }
        return ret;
    }
}

// X X X [X X X i] X X X
//         + <- +
//         - <- -
// dp[i] = dp[i-1] * nums[i], nums[i]
```

```java
// Java 1ms(Beats 85.75%), Time O(N), Space O(1)
class Solution {
    public int maxProduct(int[] nums) {
        int n = nums.length;
        int left = 1, right = 1;
        int ret = nums[0];

        for (int i = 0; i < n; i++) {

            //if any of left or right become 0 then update it to 1
            left = left == 0 ? 1 : left;
            right = right == 0 ? 1 : right;

            left *= nums[i];
            right *= nums[n - 1 - i];

            ret = Math.max(ret, Math.max(left, right));
        }
        return ret;
    }
}

```
---

[wisdompeak/YouTube解說](https://www.youtube.com/watch?v=LQuZkqx16PU)

> 解法1

`dp[i]`為以i為結尾的subarray的最大值

因為乘法的關係, 
若nums[i]為正數, 則乘於最大的dp[i-1], 有最大值
若nums[i]為負數, 則乘於最小的dp[i-1], 有最大值 (負負得正)
因此必須求得每次的dp[i-1]之最大值與最小值, 來供後面的dp[i]使用

> 解法2

若數列有偶數個負數, 則最大值必為全數列的乘績
若數列有奇數個負數, 則最大值必為捨去第一個負數的乘積, 或捨去最後一個負數的乘積

###### tags: `Leetcode` `Dynamic Programming` `Maximum Subarray`