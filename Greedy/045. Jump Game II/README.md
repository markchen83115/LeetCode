# Leetcode - 045. Jump Game II (M)

[Leetcode](https://leetcode.com/problems/jump-game-ii/)

You are given a **0-indexed** array of integers `nums` of length `n`. You are initially positioned at `nums[0]`.

Each element `nums[i]` represents the maximum length of a forward jump from index `i`. In other words, if you are at `nums[i]`, you can jump to any `nums[i + j]` where:

-   `0 <= j <= nums[i]` and
-   `i + j < n`

Return _the minimum number of jumps to reach _`nums[n - 1]`. The test cases are generated such that you can reach `nums[n - 1]`.

**Example 1:**

> **Input:** nums = [2,3,1,1,4]
> **Output:** 2
> **Explanation:** The minimum number of jumps to reach the last index is 2. Jump 1 step from index 0 to 1, then 3 steps to the last index.

**Example 2:**

> **Input:** nums = [2,3,0,1,4]
> **Output:** 2

**Constraints:**

-   `1 <= nums.length <= 104`
-   `0 <= nums[i] <= 1000`
-   It's guaranteed that you can reach `nums[n - 1]`.

---
```java
// Java 2ms(Beats 36.24%), Time O(N), Space O(1)
class Solution {
    public int jump(int[] nums) {
        int start = 0, end = 0;
        int newEnd = 0;
        int jumps = 0;
        while (start <= end)
        {
            if (start == nums.length - 1)
                return jumps;

            newEnd = Math.max(newEnd, start + nums[start]);
            if (start == end)
            {
                jumps++;
                end = newEnd;
            }
            start++;
        }

        return 0;
    }
}

// Java 0ms(Beats 100.00%), Time O(N), Space O(1)
class Solution {
    public int jump(int[] nums) {
		return jump(nums, 0, 0);
	}
	
	private int jump(int[] nums, int startIndex, int endIndex) {
		if (endIndex >= nums.length - 1) {
			return 0;
		}
		
		int maxIndex = 0;
		endIndex = Math.min(endIndex, nums.length - 1);
		
		for (int i = startIndex; i <= endIndex; i++) {
			maxIndex = Math.max(maxIndex, i + nums[i]);
		}
		
		return jump(nums, endIndex + 1, maxIndex) + 1;
	}
}
```
---

過程像是BFS
`x (aaa) (bbbbbb) (ccc)`
當在`x`時, 最遠可以走到`(aaa)的end`
到達`(aaa)`時, 代表往前跳了一步

在`(aaa)`時, 最遠可以走到`(bbbbbb)的end`
到達`(bbbbbb)`時, 代表往前跳了一步
...以此類推

###### tags: `Leetcode` `Greedy`