# Leetcode - 135. Candy (M+)

[Leetcode](https://leetcode.com/problems/candy/)

There are `n` children standing in a line. Each child is assigned a rating value given in the integer array `ratings`.

You are giving candies to these children subjected to the following requirements:

-   Each child must have at least one candy.
-   Children with a higher rating get more candies than their neighbors.

Return _the minimum number of candies you need to have to distribute the candies to the children_.

**Example 1:**

> **Input:** ratings = [1,0,2]
> **Output:** 5
> **Explanation:** You can allocate to the first, second and third child with 2, 1, 2 candies respectively.

**Example 2:**

> **Input:** ratings = [1,2,2]
> **Output:** 4
> **Explanation:** You can allocate to the first, second and third child with 1, 2, 1 candies respectively.
> The third child gets 1 candy because it satisfies the above two conditions.

**Constraints:**

-   `n == ratings.length`
-   `1 <= n <= 2 * 104`
-   `0 <= ratings[i] <= 2 * 104`

---
```java
// Java 3ms(Beats 83.76%), Time O(N), Space O(N)
class Solution {
    public int candy(int[] ratings) {
        int n = ratings.length;
        int[] nums = new int[n];
        int ret = 0;
        nums[0] = 1;
        for (int i = 1; i < n; i++)
        {
            if (ratings[i] > ratings[i-1])
                nums[i] = nums[i - 1] + 1;
            else
                nums[i] = 1;
        }

        for (int i = n - 2; i >= 0; i--)
            if (ratings[i] > ratings[i + 1])
                nums[i] = Math.max(nums[i], nums[i + 1] + 1);

        for (int i = 0; i < n; i++)
            ret += nums[i];
        
        return ret;
    }
}
```
---

我們使nums[0] = 1, 從左往右掃一遍, 確保nums[i]跟nums[i-1]是符合題意的
```java
if (ratings[i] > ratings[i-1])
    nums[i] = nums[i - 1] + 1;
else
    nums[i] = 1;
```

再從右到左掃一遍, 確保nums[i]跟nums[i+1]是符合題意的
```java
if (ratings[i] > ratings[i + 1])
    nums[i] = Math.max(nums[i], nums[i + 1] + 1);
```

因此確保了nums[i]跟左右兩邊都比較過, 也都符合題意


###### tags: `Leetcode` `Greedy` `Two-pass distribution`