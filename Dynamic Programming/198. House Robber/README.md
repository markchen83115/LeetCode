# Leetcode - 198. House Robber (E)

[Leetcode](https://leetcode.com/problems/house-robber/description/)

You are a professional robber planning to rob houses along a street. Each house has a certain amount of money stashed, the only constraint stopping you from robbing each of them is that adjacent houses have security systems connected and **it will automatically contact the police if two adjacent houses were broken into on the same night**.

Given an integer array `nums` representing the amount of money of each house, return _the maximum amount of money you can rob tonight **without alerting the police**_.

**Example 1:**
```
Input: nums = [1,2,3,1]
Output: 4
Explanation: Rob house 1 (money = 1) and then rob house 3 (money = 3).
Total amount you can rob = 1 + 3 = 4.
```
**Example 2:**
```
Input: nums = [2,7,9,3,1]
Output: 12
Explanation: Rob house 1 (money = 2), rob house 3 (money = 9) and rob house 5 (money = 1).
Total amount you can rob = 2 + 9 + 1 = 12.
```
**Constraints:**

-   `1 <= nums.length <= 100`
-   `0 <= nums[i] <= 400`

---
```java
// Java 0ms(Beats 100%), Time O(N), Space O(1) 
class Solution {
    public int rob(int[] nums) {
        if (nums.length == 0) 
            return 0;
        
        int rob = nums[0], unrob = 0;
        for (int i = 1; i < nums.length; i++) {
            int tmp_rob = rob, tmp_unrob = unrob;
            unrob = Math.max(tmp_rob, tmp_unrob);
            rob = tmp_unrob + nums[i];
        }
        return Math.max(rob, unrob);
    }
}
/*
rob 3    -> unrob 4
unrob 3  -> rob 4
unrob 3  -> unrob 4

unrob 4 = Max(rob 3, unrob 3);
rob 4 = unrob 3 + nums[4];
*/
```

---

[wisdompeak YouTube解說](https://www.youtube.com/watch?v=IcEX9Oi_tao)

###### tags: `Leetcode` `Dynamic Programming` `基本型 I`