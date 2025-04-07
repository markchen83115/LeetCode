# Leetcode - 368. Largest Divisible Subset (M+)

[Leetcode](https://leetcode.com/problems/largest-divisible-subset/)

Given a set of **distinct** positive integers `nums`, return the largest subset `answer` such that every pair `(answer[i], answer[j])` of elements in this subset satisfies:

-   `answer[i] % answer[j] == 0`, or
-   `answer[j] % answer[i] == 0`

If there are multiple solutions, return any of them.

**Example 1:**

> **Input:** nums = [1,2,3]
> **Output:** [1,2]
> **Explanation:** [1,3] is also accepted.

**Example 2:**

> **Input:** nums = [1,2,4,8]
> **Output:** [1,2,4,8]

**Constraints:**

-   `1 <= nums.length <= 1000`
-   `1 <= nums[i] <= 2 * 109`
-   All the integers in `nums` are **unique**.

---
```java
// Java 14ms(Beats 66.53%), Time O(N^2), Space O(N) 
class Solution {
    public List<Integer> largestDivisibleSubset(int[] nums) {
        int n = nums.length;
        int[] dp = new int[n];  // dp[i]: consider nums[0:i], Largest Divisible Subset Length end with nums[i]
        Arrays.fill(dp, 1);
        int maxLen = 0;
        int maxIndex = 0;

        int[] prev = new int[n];
        Arrays.fill(prev, -1);

        Arrays.sort(nums);
        for (int i = 1; i < n; i++)
        {
            for (int j = 0; j < i; j++)
                if (nums[i] % nums[j] == 0)
                    if (dp[j] + 1 >= dp[i])
                    {
                        dp[i] = dp[j] + 1;
                        prev[i] = j;
                    }

            if (maxLen <= dp[i])
            {
                maxLen = dp[i];
                maxIndex = i;
            }
        }

        List<Integer> ret = new ArrayList<>();
        int k = maxIndex;
        while (k >= 0)
        {
            ret.add(nums[k]);
            k = prev[k];
        }

        return ret;
    }
}
```
---

參考[【每日一题】368. Largest Divisible Subset, 4/5/2020 - YouTube](https://www.youtube.com/watch?v=hrwP6I5v1XY)

本題最直接的想法就是將nums排序之後再用DFS搜尋。但是DFS會超時。很顯然其中有非常多的重複路徑。

那是否可以透過一個visited的陣列來標記已經造訪過的元素來避免重複搜尋呢？這樣是不行的，因為按照從小到大的次序優先搜尋到的並不是最優的解。例如 1,3,5,10,30,60 這個例子，會先搜尋到 1-3-30-60，但是它不及 1-5-10-30-60. 如果30和60在第一次搜尋中已經標記為visited的的話，那麼就會錯過第二次搜尋的最優解。

於是只有考慮o(n^2)的DP演算法，思路和動態轉移方程式也是非常明確的。只不過本題不只求最長解的長度，而且要把這個最長解印出來。這樣的DP問題雖不常見，但也是很容易解決的。除了用一個DP數組記錄“狀態”外；再用一個prev數組記錄當前i元素在Largest-Divisible-Subset里之前的那個元素的位置


###### tags: `Leetcode` `Dynamic Programming` `基本型 II`