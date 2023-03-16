# Leetcode - 2584. Split the Array to Make Coprime Products (H)

[Leetcode](https://leetcode.com/problems/split-the-array-to-make-coprime-products/description/)

You are given a **0-indexed** integer array `nums` of length `n`.

A **split** at an index `i` where `0 <= i <= n - 2` is called **valid** if the product of the first `i + 1` elements and the product of the remaining elements are coprime.

-   For example, if `nums = [2, 3, 3]`, then a split at the index `i = 0` is valid because `2` and `9` are coprime, while a split at the index `i = 1` is not valid because `6` and `3` are not coprime. A split at the index `i = 2` is not valid because `i == n - 1`.

Return _the smallest index _`i`_ at which the array can be split validly or _`-1`_ if there is no such split_.

Two values `val1` and `val2` are coprime if `gcd(val1, val2) == 1` where `gcd(val1, val2)` is the greatest common divisor of `val1` and `val2`.

**Example 1:**

![](https://assets.leetcode.com/uploads/2022/12/14/second.PNG)
```
Input: nums = [4,7,8,15,3,5]
Output: 2
Explanation: The table above shows the values of the product of the first i + 1 elements, the remaining elements, and their gcd at each index i.
The only valid split is at index 2.
```
**Example 2:**

![](https://assets.leetcode.com/uploads/2022/12/14/capture.PNG)
```
Input: nums = [4,7,15,8,3,5]
Output: -1
Explanation: The table above shows the values of the product of the first i + 1 elements, the remaining elements, and their gcd at each index i.
There is no valid split.
```
**Constraints:**

-   `n == nums.length`
-   `1 <= n <= 104`
-   `1 <= nums[i] <= 106`

---
```java
// Java 348ms(53.95%), Time O(N), Space O(N)
class Solution {
    public int findValidSplit(int[] nums) {
        HashMap<Integer, Integer> totalEleMap = new HashMap();
        for (int num : nums)
            for (int j : factorize(num))
                totalEleMap.merge(j, 1, Integer::sum);
        
        HashMap<Integer, Integer> currMap = new HashMap();
        for (int i = 0, count = 0; i < nums.length - 1; i++) {
            int num = nums[i];
            for (int j : factorize(num)) {
                currMap.merge(j, 1, Integer::sum);
                if (currMap.get(j) == 1)    // first time see j
                    count++;
                if (currMap.get(j).equals(totalEleMap.get(j)))   // all j in curr
                    count--;
            }
            if (count == 0)
                return i;
        }
        return -1;
    }

    public List<Integer> factorize(int num) {
        List<Integer> list = new ArrayList<>();
        for (int j = 2; j * j <= num; j++) {
            if (num % j == 0) {
                list.add(j);
                while (num % j == 0)
                num /= j;
            }
        }
        if (num > 1)
            list.add(num);
        return list;
    }
}

```


---

###### tags: `Leetcode` `Others`
