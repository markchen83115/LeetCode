# Leetcode - 2552. Count Increasing Quadruplets (H-)

[Leetcode](https://leetcode.com/problems/count-increasing-quadruplets/description/)

Given a **0-indexed** integer array `nums` of size `n` containing all numbers from `1` to `n`, return _the number of increasing quadruplets_.

A quadruplet `(i, j, k, l)` is increasing if:

-   `0 <= i < j < k < l < n`, and
-   `nums[i] < nums[k] < nums[j] < nums[l]`.

**Example 1:**
```
Input: nums = [1,3,2,4,5]
Output: 2
Explanation: 
- When i = 0, j = 1, k = 2, and l = 3, nums[i] < nums[k] < nums[j] < nums[l].
- When i = 0, j = 1, k = 2, and l = 4, nums[i] < nums[k] < nums[j] < nums[l]. 
There are no other quadruplets, so we return 2.
```
**Example 2:**
```
Input: nums = [1,2,3,4]
Output: 0
Explanation: There exists only one quadruplet with i = 0, j = 1, k = 2, l = 3, but since nums[j] < nums[k], we return 0.
```
**Constraints:**

-   `4 <= nums.length <= 4000`
-   `1 <= nums[i] <= nums.length`
-   All the integers of `nums` are **unique**. `nums` is a permutation.

---

```java
// Java Time:O(n^2) Space:O(n^2) 711 ms
class Solution {
    public long countQuadruplets(int[] nums) {
        int n = nums.length;
        int[][] pre  = new int[n+1][n+1];
        int[][] post = new int[n+1][n+1];
        long ans = 0;
        for (int j = 1; j <= n; j++) {
            if (nums[0] < j)
                pre[0][j] = 1;
            if (nums[n-1] > j)
                post[n-1][j] = 1;
            for (int i = 1; i < n; i++)
                if (nums[i] < j) {
                    pre[i][j] = pre[i-1][j] + 1;
                } else {
                    pre[i][j] = pre[i-1][j];
                }
                    
            for (int i = n - 1; i >= 1; i--)
                if (nums[i] > j) {
                    post[i][j] = post[i+1][j] + 1;
                } else {
                    post[i][j] = post[i+1][j];
                }
        }

        for (int j = 1; j < n; j++) {
            for (int k = j + 1; k < n; k++) {
                if (nums[j] > nums[k])
                    ans += pre[j-1][nums[k]] * post[k+1][nums[j]];
            }
        }

        return ans;
    }
}
```

```java
// Java Time:O(n^2) Space:O(n) 39 ms
class Solution {
    public long countQuadruplets(int[] nums) {
        int n = nums.length;
        long[] dp = new long[n];
        long ans = 0;
        for (int j = 0; j < n; j++) {
            int prev_small = 0;
            for (int l = 0; l < j; l++) {
                if (nums[j] > nums[l]) {
                    prev_small++;
                    ans += dp[l];
                } else if (nums[j] < nums[l]) {
                    dp[l] += prev_small;
                }
            }
        }
        return ans;
    }
}
```

---

> 方法1

`pre[i][j]`  代表從index 0到i中比j小的個數
`post[i][j]` 代表從index n-1到i中比j大的個數
答案為將j, k固定, 將i與l的個數相乘之總和

> 方法2

來源[[C++|Java|Python3] Clean DP with Clarification O(n^2)](https://leetcode.com/problems/count-increasing-quadruplets/solutions/3111697/c-java-python3-clean-dp-with-clarification-o-n-2/)

`dp[j]` stores the **count** of all valid triplets `(i, j, k)` that satisfies `i < j < k` and `nums[i] < nums[k] < nums[j]` and using the current number as the role `j`. (Refer to [Leetcode Q456 132 pattern](https://leetcode.com/problems/132-pattern/), point out by @RealFan.)

For every new value `l`, iterate all previous stored `dp[j]` (132 pattern counts). If `nums[l] > nums[j]`, they can form a valid 1324 quadruplet pattern, then add `dp[j]` into total 1324 counts.

During iteration, we also update previous `dp[j]` by keeping track of the amount of smaller numbers in front of each new value. If `nums[l] < nums[j]`, the new value is a potential `k` for `j` in the future, so add its `previous_small` to the `dp[j]`.

![image.png](https://assets.leetcode.com/users/images/82dfc15a-8118-4c37-a77c-86183b88b6c8_1675021651.999115.png)




###### tags: `Leetcode` `Dynamic Programming` `Enumeration`
