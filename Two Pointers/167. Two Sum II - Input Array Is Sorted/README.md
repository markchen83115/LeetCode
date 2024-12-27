# Leetcode - 167. Two Sum II - Input Array Is Sorted (E+)

[Leetcode](https://leetcode.com/problems/two-sum-ii-input-array-is-sorted/)

Given a **1-indexed** array of integers `numbers` that is already **_sorted in non-decreasing order_**, find two numbers such that they add up to a specific `target` number. Let these two numbers be `numbers[index1]` and `numbers[index2]` where `1 <= index1 < index2 <= numbers.length`.

Return_ the indices of the two numbers, _`index1`_ and _`index2`_, **added by one** as an integer array _`[index1, index2]`_ of length 2._

The tests are generated such that there is **exactly one solution**. You **may not** use the same element twice.

Your solution must use only constant extra space.

**Example 1:**

> **Input:** numbers = [2,7,11,15], target = 9
> **Output:** [1,2]
> **Explanation:** The sum of 2 and 7 is 9. Therefore, index1 = 1, index2 = 2. We return [1, 2].

**Example 2:**

> **Input:** numbers = [2,3,4], target = 6
> **Output:** [1,3]
> **Explanation:** The sum of 2 and 4 is 6. Therefore index1 = 1, index2 = 3. We return [1, 3].

**Example 3:**

> **Input:** numbers = [-1,0], target = -1
> **Output:** [1,2]
> **Explanation:** The sum of -1 and 0 is -1. Therefore index1 = 1, index2 = 2. We return [1, 2].

**Constraints:**

-   `2 <= numbers.length <= 3 * 104`
-   `-1000 <= numbers[i] <= 1000`
-   `numbers` is sorted in **non-decreasing order**.
-   `-1000 <= target <= 1000`
-   The tests are generated such that there is **exactly one solution**.

---
```java
// Java 2ms(Beats 93.33%), Time O(N), Space O(1)
class Solution {
    public int[] twoSum(int[] numbers, int target) {
        int i = 0, j = numbers.length - 1;
        while (i < j)
        {
            int sum = numbers[i] + numbers[j];
            if (sum == target)
                return new int[] {i + 1, j + 1};
            
            if (sum < target)
                i++;
            else
                j--;
        }

        return new int[] {i, j};
    }
}
```
---

因為`numbers`已經排好序, 因此只需要將`i`, `j`分別從頭尾開始
若`numbers[i] + numbers[j] < target`, 則代表需將`number[i]`往上移動, 取得更大的值
反之則代表需將`number[j]`往下移動, 取得更小的值


###### tags: `Leetcode` `Two Pointers`