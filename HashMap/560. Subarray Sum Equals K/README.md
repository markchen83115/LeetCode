# Leetcode - 560. Subarray Sum Equals K (M)

[Leetcode](https://leetcode.com/problems/subarray-sum-equals-k/)

Given an array of integers `nums` and an integer `k`, return _the total number of subarrays whose sum equals to_ `k`.

A subarray is a contiguous **non-empty** sequence of elements within an array.

**Example 1:**
```
Input: nums = [1,1,1], k = 2  
Output: 2
```
**Example 2:**
```
Input: nums = [1,2,3], k = 3  
Output: 2
```
**Constraints:**

-   `1 <= nums.length <= 2 * 104`
-   `-1000 <= nums[i] <= 1000`
-   `-107 <= k <= 107`

---

```java
// Java  
class Solution {  
    public int subarraySum(int[] nums, int k) {  
        int presum = 0, total = 0;  
        HashMap<Integer, Integer> map = new HashMap<>();  
        map.put(0, 1);  //equals 0, counts = 1  
  
        for (int i = 0; i < nums.length; i++) {  
            presum += nums[i];  
            total += map.getOrDefault(presum - k, 0);  
            map.put(presum, map.getOrDefault(presum, 0) + 1);  
        }  
        return total;  
    }  
}
```

```csharp
// C#  
public class Solution {  
    public int SubarraySum(int[] nums, int k) {  
        int presum = 0, total = 0;  
        Dictionary<int, int> dict = new Dictionary<int, int>();  
        dict.Add(0, 1);  //equals 0, counts = 1  
  
        for (int i = 0; i < nums.Length; i++) {  
            presum += nums[i];  
            total += dict.GetValueOrDefault(presum - k, 0);  
            if (dict.ContainsKey(presum)) {  
                dict[presum] += 1;  
            } else {  
                dict.Add(presum, 1);  
            }  
              
        }  
        return total;  
    }  
}
```

---

利用Prefix Sum  
當`nums[i]的presum = n`時, 則檢查`presum = n — k` 的個數,  
代表從subarray會有多少個相加為k, 因為`presum[i] - presum[j] = k`


###### tags: `Leetcode` `HashMap` `Hash+Prefix`
