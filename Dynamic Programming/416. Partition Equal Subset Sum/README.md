# Leetcode - 416. Partition Equal Subset Sum (M+)

[Leetcode](https://leetcode.com/problems/partition-equal-subset-sum/)

Given an integer array `nums`, return `true` _if you can partition the array into two subsets such that the sum of the elements in both subsets is equal or _`false`_ otherwise_.

**Example 1:**
```
Input: nums = [1,5,11,5]
Output: true
Explanation: The array can be partitioned as [1, 5, 5] and [11].
```
**Example 2:**
```
Input: nums = [1,2,3,5]
Output: false
Explanation: The array cannot be partitioned into equal sum subsets.
```
**Constraints:**

-   `1 <= nums.length <= 200`
-   `1 <= nums[i] <= 100`

---
```java
// Java 13ms (Beats 97.22%), Time O(M*N), Space O(N)
class Solution {
    public boolean canPartition(int[] nums) {
        int sum = 0;
        for (int num : nums)
            sum += num;
        if ((sum & 1) == 1) return false;

        boolean[] dp = new boolean[sum / 2 + 1];
        dp[0] = true;
        for (int num : nums)
            for (int i = sum / 2; i >= 0; i--) {
                if (i + num <= sum / 2 && dp[i] == true)
                    dp[i + num] = true;
            }

        return dp[sum / 2];
    }
}
```
```java
// Java 13ms (Beats 97.22%), Time O(M*N), Space O(N)
class Solution {
    public boolean canPartition(int[] nums) {
        int sum = 0;
        for (int num : nums)
            sum += num;
        if ((sum & 1) == 1)
            return false;
        
        sum >>= 1;

        int n = nums.length;
        boolean[] dp = new boolean[sum + 1]; // dp[i] = total is j
        dp[0] = true;
        for (int num : nums)
            for (int j = sum - num; j >= 0; j--)
            {
                if (dp[j])
                    dp[j + num] = true;

                if (dp[sum])
                    return true;
            }
        
        return false;

    }
}
```
---

[wisdompeak/YouTube](https://www.youtube.com/watch?v=m1mBZjtvfiA)
講解01背包問題的模板
```java
for (int num : nums) {	//遍歷物品
	for (int s = 0; s <= sum/2; s++) {	//遍歷容量

	}
}
```

完全背包問題的模板 (與01背包問題相反)
```java
for (int capacity = 0; capacity <= n; capacity++) {	//遍歷容量
	for (int num : nums) {	//遍歷物品


	}
}
```

###### tags: `Leetcode` `Dynamic Programming` `背包型`