# Leetcode - 1658. Minimum Operations to Reduce X to Zero (M)

[Leetcode](https://leetcode.com/problems/minimum-operations-to-reduce-x-to-zero/)

You are given an integer array `nums` and an integer `x`. In one operation, you can either remove the leftmost or the rightmost element from the array `nums` and subtract its value from `x`. Note that this **modifies** the array for future operations.

Return _the **minimum number** of operations to reduce _`x` _to **exactly**_ `0` _if it is possible__, otherwise, return _`-1`.

**Example 1:**

> **Input:** nums = [1,1,4,2,3], x = 5
> **Output:** 2
> **Explanation:** The optimal solution is to remove the last two elements to reduce x to zero.

**Example 2:**

> **Input:** nums = [5,6,7,8,9], x = 4
> **Output:** -1

**Example 3:**

> **Input:** nums = [3,2,20,1,1,3], x = 10
> **Output:** 5
> **Explanation:** The optimal solution is to remove the last three elements and the first two elements (5 operations in total) to reduce x to zero.

**Constraints:**

-   `1 <= nums.length <= 105`
-   `1 <= nums[i] <= 104`
-   `1 <= x <= 109`

---
```java
// Java Sliding Window 5ms(Beats 50.38%), Time O(N), Space O(1)
class Solution {
    public int minOperations(int[] nums, int x) {
        int n = nums.length;
        int j = 0, ret = -1, sum = 0, target = 0;
        for (int i = 0; i < n; i++)
            target += nums[i];
        target -= x;

        for (int i = 0; i < n; i++)
        {
            while (j < n && sum + nums[j] <= target)
            {
                sum += nums[j];
                j++;
            }

            if (sum == target)
                ret = Math.max(ret, j - i);

            sum -= nums[i];
        }

        return ret == -1 ? -1 : n - ret;
    }
}
```
```java
// Java Prefix Sum 81ms(Beats 7.52%), Time O(N), Space O(N)
class Solution {
    public int minOperations(int[] nums, int x) {
        int n = nums.length;
        HashMap<Integer, Integer> prefix = new HashMap<>();
        int sum = 0;
        prefix.put(0, 0);
        for (int i = 0; i < n; i++)
        {
            sum += nums[i];
            prefix.putIfAbsent(sum, i + 1);
        }

        sum = 0;
        int ret = Math.min(n + 1, prefix.getOrDefault(x, n + 1));
        for (int i = n - 1; i >= 0; i--)
        {
            sum += nums[i];
            ret = Math.min(ret, n - i + prefix.getOrDefault(x - sum, n + 1));
        }

        return ret == n + 1 ? -1 : ret;
    }
}
```
---

#### Sliding Window

將題目看為找出最長的`substring`, 其總和為`sum - x`
答案則為`nums.length - 最長的substring`


#### Prefix Sum

先記錄所有的`Prefix Sum`與其個數
載從尾巴開始計算`Suffix Sum`, 尋找是否`Prefix Sum + Suffix Sum = x`, 並更新答案


###### tags: `Leetcode` `Two Pointers` `Sliding window` `Hash Map` `Hash+Prefix`