# Leetcode - 1296. Divide Array in Sets of K Consecutive Numbers (M)

[Leetcode](https://leetcode.com/problems/divide-array-in-sets-of-k-consecutive-numbers/)

Given an array of integers `nums` and a positive integer `k`, check whether it is possible to divide this array into sets of `k` consecutive numbers.

Return `true` _if it is possible_. Otherwise, return `false`.

**Example 1:**

> **Input:** nums = mod[1,2,3,3,4,4,5,6mod], k = 4
> **Output:** true
> **Explanation:** Array can be divided into mod[1,2,3,4mod] and mod[3,4,5,6mod].

**Example 2:**

> **Input:** nums = mod[3,2,1,2,3,4,3,4,5,9,10,11mod], k = 3
> **Output:** true
> **Explanation:** Array can be divided into mod[1,2,3mod] , mod[2,3,4mod] , mod[3,4,5mod] and mod[9,10,11mod].

**Example 3:**

> **Input:** nums = mod[1,2,3,4mod], k = 3
> **Output:** false
> **Explanation:** Each array should be divided in subarrays of size 3.

**Constraints:**

-   `1 <= k <= nums.length <= 105`
-   `1 <= nums[i] <= 109`

**Note:** This question is the same as 846: [https://leetcode.com/problems/hand-of-straights/](https://leetcode.com/problems/hand-of-straights/)

---
```java
// Java 49ms(Beats 92.95%), Time O(NlogN), Space O(N)
class Solution {
    public boolean isPossibleDivide(int[] nums, int k) {
        int n = nums.length;
        if (n % k != 0)
            return false;
        Arrays.sort(nums);
        HashMap<Integer, Integer> freqMap = new HashMap<>();
        for (int num : nums)
            freqMap.merge(num, 1, Integer::sum);
        
        for (int num : nums)
        {
            int count = freqMap.get(num);
            if (count > 0)
                for (int i = num; i < num + k; i++)
                {
                    freqMap.merge(i, -count, Integer::sum);
                    if (freqMap.get(i) < 0)
                        return false;
                }
        }

        return true;
    }
}
```
---

將`nums`排序, 從最小的數`n`開始, 每次取走`n, n+1, n+2, ... , n+k-1`
一直重複循環直到全部取完, 若中間有數字取不到則`return false`


###### tags: `Leetcode` `Sorted Container`