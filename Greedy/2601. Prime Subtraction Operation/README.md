# Leetcode - 2601. Prime Subtraction Operation (M)

[Leetcode](https://leetcode.com/problems/prime-subtraction-operation/)

You are given a **0-indexed** integer array `nums` of length `n`.

You can perform the following operation as many times as you want:

-   Pick an index `i` that you haven’t picked before, and pick a prime `p` **strictly less than** `nums[i]`, then subtract `p` from `nums[i]`.

Return _true if you can make `nums` a strictly increasing array using the above operation and false otherwise._

A **strictly increasing array** is an array whose each element is strictly greater than its preceding element.

**Example 1:**

>**Input:** nums = [4,9,6,10]
>**Output:** true
>**Explanation:** In the first operation: Pick i = 0 and p = 3, and then subtract 3 from nums[0], so that nums becomes [1,9,6,10].
>In the second operation: i = 1, p = 7, subtract 7 from nums[1], so nums becomes equal to [1,2,6,10].
>After the second operation, nums is sorted in strictly increasing order, so the answer is true.

**Example 2:**

> **Input:** nums = [6,8,11,12]
> **Output:** true
> **Explanation:** Initially nums is sorted in strictly increasing order, so we don't need to make any operations.

**Example 3:**

> **Input:** nums = [5,8,3]
> **Output:** false
> **Explanation:** It can be proven that there is no way to perform operations to make nums sorted in strictly increasing order, so the answer is false.

**Constraints:**

-   `1 <= nums.length <= 1000`
-   `1 <= nums[i] <= 1000`
-   `nums.length == n`

---
```java
// Java Time O(N), Space O(N)
class Solution {
    boolean[] prime = new boolean[1001];
    public boolean primeSubOperation(int[] nums) {
        int n = nums.length;
        for (int i = n - 2; i >= 0; i--)
        {
            if (nums[i] < nums[i + 1])
                continue;
            
            int diff = nums[i] - nums[i + 1];
            for (int k = diff + 1; k < nums[i]; k++)
            {
                if (isPrime(k))
                {
                    nums[i] -= k;
                    break;
                }
            }

            if (nums[i] >= nums[i + 1])
                return false;
        }

        return true;
    }

    boolean isPrime(int num)
    {
        if (prime[num])
            return true;
        if (num <= 1)
            return false;
        for (int i = 2; i <= Math.sqrt(num); i++)
            if (num % i == 0)
                return false;
        
        prime[num] = true;
        return true;
    }
}
```
---

題目要求數列為嚴格遞增序列, 因此由後往前看

若前一個比後一個大
則從兩者之差`nums[i] - nums[i + 1]`開始找質數,但不可比`num[i]`大

若無法找到質數, 則return false

###### tags: `Leetcode` `Greedy`