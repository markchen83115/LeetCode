# Leetcode - 287. Find the Duplicate Number (H-)

[Leetcode](https://leetcode.com/problems/find-the-duplicate-number/)

Given an array of integers `nums` containing `n + 1` integers where each integer is in the range `[1, n]` inclusive.

There is only **one repeated number** in `nums`, return _this repeated number_.

You must solve the problem **without** modifying the array `nums` and uses only constant extra space.

**Example 1:**
```
Input: nums = [1,3,4,2,2]
Output: 2
```
**Example 2:**
```
Input: nums = [3,1,3,4,2]
Output: 3
```
**Constraints:**

-   `1 <= n <= 105`
-   `nums.length == n + 1`
-   `1 <= nums[i] <= n`
-   All the integers in `nums` appear only **once** except for **precisely one integer** which appears **two or more** times.

**Follow up:**

-   How can we prove that at least one duplicate number must exist in `nums`?
-   Can you solve the problem in linear runtime complexity?

---
```java
// Java Negative Marking
// 3ms(Beats 92.87%), Time O(N), Space O(1)
class Solution {
    public int findDuplicate(int[] nums) {
        for (int i = 0; i < nums.length; i++)
        {
            int curr = Math.abs(nums[i]);
            if (nums[curr - 1] < 0) 
                return curr;
            nums[curr - 1] *= -1;
        }
        return -1;
    }
}
// [1, 3, 4, 2, 2]
// [-1, 3, 4, 2, 2]
// [-1, 3, -4, 2, 2]
// [-1, 3, -4, -2, 2]
// [-1, -3, -4, -2, 2]
```

```java
// Java Floyd's Tortoise and Hare (Cycle Detection)
// 4ms(Beats 87.70%), Time O(N), Space O(1)
class Solution {
    public int findDuplicate(int[] nums) {
        int slow = nums[0];
        int fast = nums[0];
        do
        {
            slow = nums[slow];
            fast = nums[nums[fast]];
        } while (slow != fast);

        // 此時為相遇點, 
        // 一個從相遇點出發, 一個從起點出發
        // 再次相遇點 = 環的入口
        // 142. Linked List Cycle II
        slow = nums[0];
        while (slow != fast)
        {
            slow = nums[slow];
            fast = nums[fast];
        }
        return fast;
    }
}
```


---

###### tags: `Leetcode` `Find K-th Element` `Greedy` `Indexing Sort`
