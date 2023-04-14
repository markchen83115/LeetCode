# Leetcode - 2615. Sum of Distances (M+)

[Leetcode](https://leetcode.com/problems/sum-of-distances/description/)

You are given a **0-indexed** integer array `nums`. There exists an array `arr` of length `nums.length`, where `arr[i]` is the sum of `|i - j|` over all `j` such that `nums[j] == nums[i]` and `j != i`. If there is no such `j`, set `arr[i]` to be `0`.

Return _the array _`arr`_._

**Example 1:**
```
Input: nums = [1,3,1,1,2]
Output: [5,0,3,4,0]
Explanation: 
When i = 0, nums[0] == nums[2] and nums[0] == nums[3]. Therefore, arr[0] = |0 - 2| + |0 - 3| = 5. 
When i = 1, arr[1] = 0 because there is no other index with value 3.
When i = 2, nums[2] == nums[0] and nums[2] == nums[3]. Therefore, arr[2] = |2 - 0| + |2 - 3| = 3. 
When i = 3, nums[3] == nums[0] and nums[3] == nums[2]. Therefore, arr[3] = |3 - 0| + |3 - 2| = 4. 
When i = 4, arr[4] = 0 because there is no other index with value 2. 
```
**Example 2:**
```
Input: nums = [0,5,3]
Output: [0,0,0]
Explanation: Since each element in nums is distinct, arr[i] = 0 for all i.
```
**Constraints:**

-   `1 <= nums.length <= 105`
-   `0 <= nums[i] <= 109`

---
```java
// Java 76ms(30.2%), Time O(N), Space O(N)
class Solution {
    public long[] distance(int[] nums) {
        int n = nums.length;
        long[] ret = new long[n];
        HashMap<Integer, Long> countMap = new HashMap<>();
        HashMap<Integer, Long> sumMap = new HashMap<>();
        for (int i = 0; i < n; i++) {
            sumMap.merge(nums[i], (long)i, Long::sum);
            countMap.merge(nums[i], 1L, Long::sum);
            ret[i] = i * countMap.get(nums[i]) - sumMap.get(nums[i]);
        }

        countMap.clear();
        sumMap.clear();

        for (int i = n - 1; i >= 0; i--) {
            sumMap.merge(nums[i], (long)i, Long::sum);
            countMap.merge(nums[i], 1L, Long::sum);
            ret[i] +=  sumMap.get(nums[i]) - i * countMap.get(nums[i]);
        }

        return ret;
    }
}
```

```java
// Java 27ms(97.51%), 
class Solution {
    public long[] distance(int[] nums) {
        int n = nums.length;
        long[] ret = new long[n];
        HashMap<Integer, List<Integer>> map = new HashMap<>();
        for (int i = 0; i < n; i++)
            map.computeIfAbsent(nums[i], x -> new ArrayList<Integer>()).add(i); // put index

        for (List<Integer> lst : map.values()) {
            int m = lst.size();
            long[] sum = new long[m + 1];
            for (int i = 0; i < m; i++)
                sum[i + 1] = sum[i] + lst.get(i);   // compute prefix sum
            for (int i = 0; i < m; i++) {
                int curr = lst.get(i);
                long left = (long) i * curr - sum[i];
                long right = sum[m] - sum[i] - (long) (m - i) * curr;
                ret[curr] = left + right;
            }
        }
        return ret;
    }
}
```
---

計算絕對值總和時, 可利用提前加總, 從O(N)達到O(1)的效果
```java
Consider 1's at different postions of an array. 
x  y  z   p   q
1  1  1   1   1

consider 1 at index z: |z - x| + |z - y| + |z - p| + |z - q|

when we are looping from left to right we are storing sum and count of previous indices of num in maps.
|z - x| + |z - y| = z - x + z - y, since z is greater than x and y.
z - x + z - y = 2z - (x + y) = (count) * (currentIndex) - (sum).

Similarly we can calculate the |z - p| + |z - q| when we loop from right to left.
```


###### tags: `Leetcode` `Hash+Prefix`