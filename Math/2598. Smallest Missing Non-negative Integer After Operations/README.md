# Leetcode - 2598. Smallest Missing Non-negative Integer After Operations (M-)

[Leetcode](https://leetcode.com/problems/smallest-missing-non-negative-integer-after-operations/description/)

You are given a **0-indexed** integer array `nums` and an integer `value`.

In one operation, you can add or subtract `value` from any element of `nums`.

-   For example, if `nums = [1,2,3]` and `value = 2`, you can choose to subtract `value` from `nums[0]` to make `nums = [-1,2,3]`.

The MEX (minimum excluded) of an array is the smallest missing **non-negative** integer in it.

-   For example, the MEX of `[-1,2,3]` is `0` while the MEX of `[1,0,3]` is `2`.

Return _the maximum MEX of _`nums`_ after applying the mentioned operation **any number of times**_.

**Example 1:**
```
Input: nums = [1,-10,7,13,6,8], value = 5
Output: 4
Explanation: One can achieve this result by applying the following operations:
- Add value to nums[1] twice to make nums = [1,0,7,13,6,8]
- Subtract value from nums[2] once to make nums = [1,0,2,13,6,8]
- Subtract value from nums[3] twice to make nums = [1,0,2,3,6,8]
The MEX of nums is 4. It can be shown that 4 is the maximum MEX we can achieve.
```
**Example 2:**
```
Input: nums = [1,-10,7,13,6,8], value = 7
Output: 2
Explanation: One can achieve this result by applying the following operation:
- subtract value from nums[2] once to make nums = [1,-10,0,13,6,8]
The MEX of nums is 2. It can be shown that 2 is the maximum MEX we can achieve.
```
**Constraints:**

-   `1 <= nums.length, value <= 105`
-   `-109 <= nums[i] <= 109`

---
```java
// Java 比賽答案70ms, Time O(N), Space O(N) 
class Solution {
    public int findSmallestInteger(int[] nums, int value) {
        HashMap<Integer, Integer> map = new HashMap<>();
        int n = nums.length;
        for (int i = 0; i < n; i++) {
            nums[i] %= value;
            if (nums[i] < 0)
                nums[i] += value;
            map.merge(nums[i], 1, Integer::sum);
        }
        int p = 0;
        for (int i = 0; i < n; i++) {
            if (map.getOrDefault(p, 0) == 0) {
                return i;
            }
            map.put(p, map.get(p) - 1);
            p++;
            p %= value;
            
        }
        return n;
    }
}
```

```java
// Java 7ms
class Solution {
    public int findSmallestInteger(int[] nums, int value) {
        int[] count = new int[value];
        int n = nums.length;
        for (int num : nums)
            count[(num % value + value) % value]++;

        int stop = 0;
        for (int i = 0; i < value; i++) {
            if (count[i] < count[stop])
                stop = i;
        }
        return value * count[stop] + stop;
    }
}
```

---

要尋找`0 ~ n`的最小缺值, 則可利用每個餘數的個數,
因為產生從`[0] ~ [value - 1]`需要每個餘數都貢獻一次,
擁有最少個數的餘數其貢獻次數無法跟上其他餘數, 必然會產生最小缺值

###### tags: `Leetcode` `Math`