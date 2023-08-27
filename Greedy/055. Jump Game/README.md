# Leetcode - 55. Jump Game (E+)

[Leetcode](https://leetcode.com/problems/jump-game/description/)

You are given an integer array `nums`. You are initially positioned at the array's **first index**, and each element in the array represents your maximum jump length at that position.

Return _`true`_ if you can reach the last index, or _`false`_ otherwise.

**Example 1:**
```
Input: nums = [2,3,1,1,4]
Output: true
Explanation: Jump 1 step from index 0 to 1, then 3 steps to the last index.
```
**Example 2:**
```
Input: nums = [3,2,1,0,4]
Output: false
Explanation: You will always arrive at index 3 no matter what. Its maximum jump length is 0, which makes it impossible to reach the last index.
```
**Constraints:**

-   `1 <= nums.length <= 104`
-   `0 <= nums[i] <= 105`

---

```java
// Java 2ms(Beats 80.26%), Time O(N), Space O(1)
class Solution {
    public boolean canJump(int[] nums) {
        int n = nums.length;
        int max = 0;
        for (int i = 0; i < n; i++) {
            if (max < i) return false;
            max = Math.max(max, i + nums[i]);
        }
        return true;
    }
}
```

---
一個一個考察`nums[i]`, 並計算當前能到達的最遠距離,
若能到達的最遠距離<`i`, 代表不可能到達終點


###### tags: `Leetcode` `Greedy`