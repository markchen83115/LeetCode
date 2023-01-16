# Leetcode - 2537. Count the Number of Good Subarrays (M+)

[Leetcode](https://leetcode.com/problems/count-the-number-of-good-subarrays/description/)

Given an integer array `nums` and an integer `k`, return _the number of **good** subarrays of_ `nums`.

A subarray `arr` is **good** if it there are **at least **`k` pairs of indices `(i, j)` such that `i < j` and `arr[i] == arr[j]`.

A **subarray** is a contiguous **non-empty** sequence of elements within an array.

**Example 1:**
```
Input: nums = [1,1,1,1,1], k = 10
Output: 1
Explanation: The only good subarray is the array nums itself.
```
**Example 2:**
```
Input: nums = [3,1,4,3,2,2,4], k = 2
Output: 4
Explanation: There are 4 different good subarrays:
- [3,1,4,3,2,2] that has 2 pairs.
- [3,1,4,3,2,2,4] that has 3 pairs.
- [1,4,3,2,2,4] that has 2 pairs.
- [4,3,2,2,4] that has 2 pairs.
```
**Constraints:**

-   `1 <= nums.length <= 105`
-   `1 <= nums[i], k <= 109`

---

```java
// Java
class Solution {
    public long countGood(int[] nums, int k) {
        HashMap<Integer, Integer> map = new HashMap<>();
        int j = 0, n = nums.length;
        long pair = 0, ret = 0;
        for (int i = 0; i < n; i++) {
            while (pair < k && j < n) {
                int e = map.getOrDefault(nums[j], 0);
                pair += e;
                map.put(nums[j], e + 1);
                j++;
            }
            if (pair >= k)
                ret += n - j + 1;
                
            int q = map.get(nums[i]) - 1;
            pair -= q;
            map.put(nums[i], q);
        }
        return ret;
    }
}
```

---

```
x [x x x x]  x x x 
   i     j-1 j   n-1
```
用sliding window將`j`不斷往右滑, 直到找出good subarray,
因為`j++`的緣故, 代表從`j - 1`到`n - 1`都是可以的good subarray, 
因此以`i`為頭的good subarray數量為 j - n + 1,

此時再將`i`往右滑一格縮減subarray, 若同樣為good subarray, 則再加上`j - n + 1`
若不是good subarray, 則將j繼續向右滑直到找到good subarray


###### tags: `Leetcode` `Sliding window : Distinct Characters`