# Leetcode - 930. Binary Subarrays With Sum (M)

[Leetcode](https://leetcode.com/problems/binary-subarrays-with-sum/)

Given a binary array `nums` and an integer `goal`, return _the number of non-empty **subarrays** with a sum_ `goal`.

A **subarray** is a contiguous part of the array.

**Example 1:**

> **Input:** nums = [1,0,1,0,1], goal = 2
> **Output:** 4
> **Explanation:** The 4 subarrays are bolded and underlined below:
> [**1,0,1**,0,1]
> [**1,0,1,0**,1]
> [1,**0,1,0,1**]
> [1,0,**1,0,1**]

**Example 2:**

> **Input:** nums = [0,0,0,0,0], goal = 0
> **Output:** 15

**Constraints:**

-   `1 <= nums.length <= 3 * 104`
-   `nums[i]` is either `0` or `1`.
-   `0 <= goal <= nums.length`

---
```java
// Java PrefisSum 4ms(Beats 43.10%), Time O(N), Space O(N)
class Solution {
    public int numSubarraysWithSum(int[] nums, int goal) {
        int[] prefix = new int[100005];
        prefix[0] = 1;
        int sum = 0;
        int ret = 0;
        for (int num : nums)
        {
            sum += num;
            if (sum >= goal)
                ret += prefix[sum - goal];
            
            prefix[sum]++;
        }

        return ret;
    }
}
```
```java
// Java SlidingWindow+預處理 2ms(Beats 61.01%), Time O(N), Space O(N)
class Solution {
    public int numSubarraysWithSum(int[] nums, int goal) {
        int n = nums.length;
        int[] postZero = new int[n];
        int count = 0;
        for (int i = n - 1; i >= 0; i--)
        {
            postZero[i] = count;
            if (nums[i] == 0)
                count++;
            else
                count = 0;
        }

        int ret = 0;
        int sum = 0;
        for (int i = 0, j = 0; i < n; i++)
        {
            while (j <= i || (j < n && sum < goal))
            {
                sum += nums[j];
                j++;
            }

            if (sum == goal)
                ret += 1 + postZero[j - 1];

            sum -= nums[i];
        }

        return ret;
    }
}
```
```java
// Java SlidingWindow 2ms(Beats 61.01%), Time O(N), Space O(N)
class Solution {
    public int numSubarraysWithSum(int[] nums, int goal) {
        return helper(nums, goal) - helper(nums, goal - 1);
    }

    int helper(int[] nums, int goal)    // number of subarray that sum is <= goal
    {
        int n = nums.length;
        int ret = 0;
        int sum = 0;
        for (int i = 0, j = 0; i < n; i++)  // j...i
        {
            sum += nums[i];

            while (j <= i && sum > goal)
                sum -= nums[j++];
            
            ret += i - j + 1;
        }

        return ret;
    }
}
```
---

#### 解法1：Hash+prefix

我們遍歷每一個元素j，考察以j為結尾、滿足條件的subarray，這樣的起點i可以在哪裡？如果滿足條件的起點i有多種可能，那麼答案就可以累積這麼多數量．

如何確定i的位置呢？凡是涉及到數組的subarray的和，我們通常會轉化為前綴和來處理。即`sum[i:j] = prefix[j]-prefix[i-1]`。其中sum[i:j]即使S，當我們固定j的時候，prefix[j]也是已知的。因此可以知道我們期望的prefix[i-1]是多少，假設為val。所以我們可以用Hash來儲存某個所有字首和val所對應的i的個數。因此我們就有

```
ret += Map[prefix[j] - S]

```

#### 解法2：Sliding Window

遍歷左邊界左邊界。假設左邊界為i，那麼可以向右單調移動j直到滑窗內的元素和恰好為S。此時如果知道j右邊有k個連續的0，那就意味著以i為左邊界、元素和是S的滑窗就有k+1個。

對於每個元素，它後面有多少個連續的0，可以事先預處理得到的。

因為i和j都是單調移動的，所以時間複雜度是O(N).


#### 解法3：Sliding Window 2

[靈茶山艾府](https://leetcode.cn/discuss/post/0viNMK/)
要計算有多少個元素和剛好等於 `k` 的子數組，可以把問題變成：

計算有多少個元素和 `≥k` 的子數組。
計算有多少個元素和 `>k`，也就是 `≥k+1` 的子數組。
答案是元素和 `≥k` 的子數組個數，減去元素和 `≥k+1` 的子數組個數。這裡把 `>` 轉換成 `≥`，因此可以把滑窗邏輯封裝成一個函數 `f`，然後用 `f(k) - f(k + 1)` 計算，無需寫兩份滑窗程式碼。



###### tags: `Leetcode` `HashMap` `Hash+Prefix` `Two Pointers` `Sliding window`