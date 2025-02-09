# Leetcode - 1004. Max Consecutive Ones III (M)

[Leetcode](https://leetcode.com/problems/max-consecutive-ones-iii/)

Given a binary array `nums` and an integer `k`, return _the maximum number of consecutive _`1`_'s in the array if you can flip at most_ `k` `0`'s.

**Example 1:**

> **Input:** nums = [1,1,1,0,0,0,1,1,1,1,0], k = 2
> **Output:** 6
> **Explanation:** [1,1,1,0,0,**1**,1,1,1,1,**1**]
> Bolded numbers were flipped from 0 to 1. The longest subarray is underlined.

**Example 2:**

> **Input:** nums = [0,0,1,1,0,0,1,1,1,0,1,1,0,0,0,1,1,1,1], k = 3
> **Output:** 10
> **Explanation:** [0,0,1,1,**1**,**1**,1,1,1,**1**,1,1,0,0,0,1,1,1,1]
> Bolded numbers were flipped from 0 to 1. The longest subarray is underlined.

**Constraints:**

-   `1 <= nums.length <= 105`
-   `nums[i]` is either `0` or `1`.
-   `0 <= k <= nums.length`

---
```java
// Java 4ms(Beats 26.70%), Time O(N), Space O(1)
class Solution {
    public int longestOnes(int[] nums, int k) {
        int ret = 0, j = 0, zero = 0;
        int n = nums.length;
        for (int i = 0; i < n; i++)
        {
            while (j < n && (nums[j] == 1 || nums[j] == 0 && zero < k))
            {
                if (nums[j] == 0)
                    zero++;
                j++;
            }

            ret = Math.max(ret, j - i);

            if (nums[i] == 0)
                zero--;
        }

        return ret;
    }
}
```
---

對於任何求subarray的問題，我們通常的做法就是固定左邊界，探索右邊界。假設我們固定左邊界是`i`，那麼要讓右邊界`j`最遠，需要滿足[i,j]最多有K個0。我們只需要將`j`單調右移，同時記錄中間遇到了幾個0即可

綜上，我們用for迴圈遍歷左邊界`i`：對於每個`i`我們記錄移動右指針`j`，將`j`停在count為K的最遠位置。然後左移一個`i`並更新count（如果A[i]原本是0的話，我們要吐出一個flip的名額），再接著移動`j`的位置

所以兩個指針都只會朝一個方向移動。這是快慢類型的雙指針，時間複雜度就是o(N)


###### tags: `Leetcode` `Two Pointers` `Sliding window`