# Leetcode - 2501. Longest Square Streak in an Array (E+)

[Leetcode](https://leetcode.com/problems/longest-square-streak-in-an-array/)

You are given an integer array `nums`. A subsequence of `nums` is called a **square streak** if:

-   The length of the subsequence is at least `2`, and
-   **after** sorting the subsequence, each element (except the first element) is the **square** of the previous number.

Return_ the length of the **longest square streak** in _`nums`_, or return _`-1`_ if there is no **square streak**._

A **subsequence** is an array that can be derived from another array by deleting some or no elements without changing the order of the remaining elements.

**Example 1:**

> **Input:** nums = [4,3,6,16,8,2]
> **Output:** 3
> **Explanation:** Choose the subsequence [4,16,2]. After sorting it, it becomes [2,4,16].
> - 4 = 2 * 2.
> - 16 = 4 * 4.
> Therefore, [4,16,2] is a square streak.
> It can be shown that every subsequence of length 4 is not a square streak.

**Example 2:**

> **Input:** nums = [2,3,5,6,7]
> **Output:** -1
> **Explanation:** There is no square streak in nums so return -1.

**Constraints:**

-   `2 <= nums.length <= 105`
-   `2 <= nums[i] <= 105`

---
```java
// Java DP 39ms(Beats 84.38%), Time O(N), Space O(N)
class Solution {
    public int longestSquareStreak(int[] nums) {
        int[] dp = new int[100005];
        int ret = 0;
        Arrays.sort(nums);
        for (int num : nums)
        {
            int k = (int) Math.sqrt(num);
            if (k * k == num && dp[k] > 0)
                dp[num] = Math.max(1, dp[k]) + 1;
            else
                dp[num] = 1;
        }

        for (int count : dp)
            ret = Math.max(ret, count);

        return ret >= 2 ? ret : -1;
    }
}
```
```java
// Java Hash 64ms(Beats 12.50%), Time O(N), Space O(N)
class Solution {
    public int longestSquareStreak(int[] nums) {
        Arrays.sort(nums);
        int n = nums.length;
        HashSet<Integer> set = new HashSet<>();
        for (int num : nums)
            set.add(num);
        
        int ret = 0;
        for (int k : nums)
        {
            int count = 0;
            while (set.contains(k))
            {
                set.remove(k);
                count++;
                k *= k;
            }
            ret = Math.max(ret, count);
        }

        if (ret <= 1)
            return -1;
            
        return ret;

    }
}
```
---

以`dp[i]`為最大平方數列長度
由小到大, 依序檢查`i`是否可被開根號, 且平方根需在數列內
將因此`dp[i] = dp[平方根] + 1`


###### tags: `Leetcode` `Dynamic Programming`