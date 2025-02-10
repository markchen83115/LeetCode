# Leetcode - 2831. Find the Longest Equal Subarray (M)

[Leetcode](https://leetcode.com/problems/find-the-longest-equal-subarray/)

You are given a **0-indexed** integer array `nums` and an integer `k`.

A subarray is called **equal** if all of its elements are equal. Note that the empty subarray is an **equal** subarray.

Return _the length of the **longest** possible equal subarray after deleting **at most** _`k`_ elements from _`nums`.

A **subarray** is a contiguous, possibly empty sequence of elements within an array.

**Example 1:**

> **Input:** nums = [1,3,2,3,1,3], k = 3
> **Output:** 3
> **Explanation:** It's optimal to delete the elements at index 2 and index 4.
> After deleting them, nums becomes equal to [1, 3, 3, 3].
> The longest equal subarray starts at i = 1 and ends at j = 3 with length equal to 3.
> It can be proven that no longer equal subarrays can be created.

**Example 2:**

> **Input:** nums = [1,1,2,2,1,1], k = 2
> **Output:** 4
> **Explanation:** It's optimal to delete the elements at index 2 and index 3.
> After deleting them, nums becomes equal to [1, 1, 1, 1].
> The array itself is an equal subarray, so the answer is 4.
> It can be proven that no longer equal subarrays can be created.

**Constraints:**

-   `1 <= nums.length <= 105`
-   `1 <= nums[i] <= nums.length`
-   `0 <= k <= nums.length`

---
```java
// Java 89ms(Beats 38.30%), Time O(N), Space O(N)
class Solution {
    public int longestEqualSubarray(List<Integer> nums, int k) {
        HashMap<Integer, List<Integer>> map = new HashMap<>();
        int ret = 0;
        for (int i = 0; i < nums.size(); i++)
            map.computeIfAbsent(nums.get(i), v -> new ArrayList<>()).add(i);
        
        for (int key : map.keySet())
        {
            int j = 0;
            List<Integer> list = map.get(key);
            int n = list.size();
            for (int i = 0; i < n; i++)
            {
                while (j < n && list.get(j) - list.get(i) + 1 - (j - i + 1) <= k)
                {
                    ret = Math.max(ret, j - i + 1);
                    j++;
                }
            }
        }

        return ret;
    }
}
```
---

我們遍歷數組，收集每種不同元素出現的位置。

假設對於元素A，它出現的所有的index都放入pos數組裡。那麼對於以pos[i]為左邊界的subarray，我們向右尋找一個最遠的pos[j]，使得兩個位置之間的「非A元素」的數量恰好小於等於k，那麼就表示這個區間可以透過有限的刪除操作變成equal A的subarray。數學表達式為：

```java
if (list[j] - list[i] + 1 - (j - i + 1) <= k)
    ret = max(ret, j - i + 1);
```

顯然，隨著i的移動，j必然也是單向移動的。所以在pos數組上的快慢指標的移動，可以找出所有符合要求的區間，找出其中A元素最多的一段。

同理，處理其他的元素對應的pos數組，返回全域最大的解。


###### tags: `Leetcode` `Two Pointers` `Sliding window`