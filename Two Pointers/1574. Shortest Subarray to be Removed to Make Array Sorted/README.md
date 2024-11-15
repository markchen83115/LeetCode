# Leetcode - 1574. Shortest Subarray to be Removed to Make Array Sorted (H-)

[Leetcode](https://leetcode.com/problems/shortest-subarray-to-be-removed-to-make-array-sorted/)

Given an integer array `arr`, remove a subarray (can be empty) from `arr` such that the remaining elements in `arr` are **non-decreasing**.

Return _the length of the shortest subarray to remove_.

A **subarray** is a contiguous subsequence of the array.

**Example 1:**

> **Input:** arr = [1,2,3,10,4,2,3,5]
> **Output:** 3
> **Explanation:** The shortest subarray we can remove is [10,4,2] of length 3. The remaining elements after that will be [1,2,3,3,5] which are sorted.
> Another correct solution is to remove the subarray [3,10,4].

**Example 2:**

> **Input:** arr = [5,4,3,2,1]
> **Output:** 4
> **Explanation:** Since the array is strictly decreasing, we can only keep a single element. Therefore we need to remove a subarray of length 4, either [5,4,3,2] or [4,3,2,1].

**Example 3:**

> **Input:** arr = [1,2,3]
> **Output:** 0
> **Explanation:** The array is already non-decreasing. We do not need to remove any elements.

**Constraints:**

-   `1 <= arr.length <= 105`
-   `0 <= arr[i] <= 109`

---
```java
// Java 3ms(Beats 32.14%), Time O(N), Space O(1)
class Solution {
    public int findLengthOfShortestSubarray(int[] arr) {
        int n = arr.length;
        int j = n - 1;
        while (j > 0 && arr[j - 1] <= arr[j])
            j--;
        if (j == 0)
            return 0;
        int ret = j;

        for (int i = 0; i < n; i++)
        {
            if (i > 0 && arr[i - 1] > arr[i])
                break;
            
            while (j < n && arr[i] > arr[j])
                j++;
            
            ret = Math.min(ret, j - i - 1);
        }

        return ret;
    }
}

// 1 2 3 10 4 2 3 5
//   i        j

```
---

對於subarray的題目, 追根究底為確認左右端點
將數列拆分為三部分 `[0:i], [i+1:j-1], [j:n-1]`
因此中間`[i+1:j-1]`為我們需要刪除的subarray
我們需要滿足`[0:i]為遞增數列`, `[j:n-1]為遞增數列`, 且`arr[i] <= arr[j]`
對於`[0:i]` `[j:n-1]`的遞增數列容易找

因為兩者都是遞增數列, 透過two pointer
當`arr[i] > arr[j]`時, `j`需向右移動, 直到滿足`arr[i] <= arr[j]`
此時我們即找到`[i+1:j-1]`需要刪除的subarray


###### tags: `Leetcode` `Two Pointers`