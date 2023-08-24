# Leetcode - 300. Longest Increasing Subsequence (M+)

[Leetcode](https://leetcode.com/problems/longest-increasing-subsequence/description/)

Given an integer array `nums`, return _the length of the longest **strictly increasing** _**subsequence**_.

**Example 1:**
```
Input: nums = [10,9,2,5,3,7,101,18]
Output: 4
Explanation: The longest increasing subsequence is [2,3,7,101], therefore the length is 4.
```
**Example 2:**
```
Input: nums = [0,1,0,3,2,3]
Output: 4
```
**Example 3:**
```
Input: nums = [7,7,7,7,7,7,7]
Output: 1
```
**Constraints:**

-   `1 <= nums.length <= 2500`
-   `-104 <= nums[i] <= 104`

**Follow up:** Can you come up with an algorithm that runs in `O(n log(n))` time complexity?

---
```java
// Java DP 35ms(Beats 76.48%), Time O(N^2), Space O(N)
class Solution {
    public int lengthOfLIS(int[] nums) {
        int n = nums.length;
        int[] dp = new int[n];
        int ret = 0;

        for (int i = 0; i < n; i++) {
            dp[i] = 1;
            for (int j = 0; j < i; j++) {
                if (nums[j] < nums[i])
                    dp[i] = Math.max(dp[i], dp[j] + 1);
            }
            ret = Math.max(ret, dp[i]);
        }
        return ret;
    }
}
```

```java
// Java Greedy 2ms(Beats 99.68%), Time O(NlogN), Space O(N)
class Solution {
    public int lengthOfLIS(int[] nums) {
        int[] dp = new int[nums.length];
        int len = 0;

        for (int x : nums) {
            int i = Arrays.binarySearch(dp, 0, len, x);
            if (i < 0) i = - (i + 1);
            dp[i] = x;
            if (i == len) len++;
        }

        return len;
    }
}
```

```csharp
// C# Greedy 82ms(Beats 96.42%), Time O(NlogN), Space O(N)
public class Solution {
    public int LengthOfLIS(int[] nums) {
        List<int> lst = new List<int>();
        lst.Add(nums[0]);

        foreach (int x in nums) {
            int i = BinarySearch(lst, x);
            if (i == lst.Count)
                lst.Add(x);
            else 
                lst[i] = x;
        }
        return lst.Count;
    }

    public int BinarySearch(List<int> lst, int target) {
        int left = 0, right = lst.Count;
        while (left < right) {
            int mid = left + (right - left) / 2;
            if (lst[mid] == target)
                return mid;
            else if (lst[mid] < target)
                left = mid + 1;
            else
                right = mid; 
        }
        return left;
    }
}
```

---

[wisdompeak/YouTube解說](https://www.youtube.com/watch?v=Q6KyDl_xiIg)

###### tags: `Leetcode` `Dynamic Programming` `Greedy` `LIS`