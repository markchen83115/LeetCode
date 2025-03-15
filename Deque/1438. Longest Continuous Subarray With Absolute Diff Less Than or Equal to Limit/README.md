# Leetcode - 1438. Longest Continuous Subarray With Absolute Diff Less Than or Equal to Limit (H)

[Leetcode](https://leetcode.com/problems/longest-continuous-subarray-with-absolute-diff-less-than-or-equal-to-limit/)

Given an array of integers `nums` and an integer `limit`, return the size of the longest **non-empty** subarray such that the absolute difference between any two elements of this subarray is less than or equal to `limit`_._

**Example 1:**

> **Input:** nums = [8,2,4,7], limit = 4
> **Output:** 2 
> **Explanation:** All subarrays are: 
> [8] with maximum absolute diff |8-8| = 0 <= 4.
> [8,2] with maximum absolute diff |8-2| = 6 > 4. 
> [8,2,4] with maximum absolute diff |8-2| = 6 > 4.
> [8,2,4,7] with maximum absolute diff |8-2| = 6 > 4.
> [2] with maximum absolute diff |2-2| = 0 <= 4.
> [2,4] with maximum absolute diff |2-4| = 2 <= 4.
> [2,4,7] with maximum absolute diff |2-7| = 5 > 4.
> [4] with maximum absolute diff |4-4| = 0 <= 4.
> [4,7] with maximum absolute diff |4-7| = 3 <= 4.
> [7] with maximum absolute diff |7-7| = 0 <= 4. 
> Therefore, the size of the longest subarray is 2.

**Example 2:**

> **Input:** nums = [10,1,2,4,7,2], limit = 5
> **Output:** 4 
> **Explanation:** The subarray [2,4,7,2] is the longest since the maximum absolute diff is |2-7| = 5 <= 5.

**Example 3:**

> **Input:** nums = [4,2,2,2,4,4,2,2], limit = 0
> **Output:** 3

**Constraints:**

-   `1 <= nums.length <= 105`
-   `1 <= nums[i] <= 109`
-   `0 <= limit <= 109`

---
```java
// Java 34ms(Beats 82.24%), Time O(N), Space O(N)
class Solution {
    public int longestSubarray(int[] nums, int limit) {
        int ret = 0, n = nums.length;
        Deque<Integer> maxDeq = new ArrayDeque<>();
        Deque<Integer> minDeq = new ArrayDeque<>();
        for (int i = 0, j = 0; i < n; i++)  // j...i
        {
            while (!maxDeq.isEmpty() && nums[maxDeq.peekLast()] <= nums[i])
                maxDeq.pollLast();
            maxDeq.offerLast(i);

            while (!minDeq.isEmpty() && nums[minDeq.peekLast()] >= nums[i])
                minDeq.pollLast();
            minDeq.offerLast(i);

            while (nums[maxDeq.peekFirst()] - nums[minDeq.peekFirst()] > limit)
            {
                while (!maxDeq.isEmpty() && maxDeq.peekFirst() <= j)
                    maxDeq.pollFirst();
                while (!minDeq.isEmpty() && minDeq.peekFirst() <= j)
                    minDeq.pollFirst();
                j++;
            }

            ret = Math.max(ret, i - j + 1);
        }

        return ret;
    }
}
```
---

#### 解法1：Heap

本題如果用multiset會非常好做。固定左端點i，不斷試探右端點j能前進到哪裡。 [i,j]內的元素都放入一個multiset，自動就知道了最大值和最小值。如果滿足`mx-mn<=limit`，這段區間長度就合法。否則我們就移動一次左端點i，同時要更新這個set把nums[i]從中移出去。

#### 解法2：monotone deque

multiset是o(NlogN)的解法，如果使用單調佇列，可以最佳化到o(N)。

我們不用multset來維護最大值和最小值，而是用一個deque來維護目前區間[i,j]的最大值，用另一個deque來維護目前區間[i,j]的最小值。其中用deque來維護一個滑動視窗的最大值，就是`239.Sliding-Window-Maximum`的做法。

基本想法是：當將nums[j]加入隊列時，如果發現它比隊尾的元素還大，那麼說明此時隊尾元素可以拋棄，這是因為在未來相當一段時間內區間都會包含有j，所以最大值肯定輪不到是隊尾的那個元素。這就提示我們維護的這個deque應該是單調遞減的（因為新元素會把所有小的隊尾元素都彈出）。每次我們要找目前區間的最大值，就只要看deque的隊首元素就好。

同理，我們維護一個單調遞增的deque來取得目前區間的最小值，其中最小值也是deque的隊首元素。

注意，當j前進到區間[i,j]無法滿足`mx-mn<=limit`時，j的前進就停止，我們要移動i。因此需要將nums[i]從這兩個deque中移出。移出的操作就是看隊首元素（的index）是否就是i，是的話把這個隊首元素彈出就行。同時記得更新mx和mn。



###### tags: `Leetcode` `Deque`