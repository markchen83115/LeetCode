# Leetcode - 238. Product of Array Except Self (M)

[Leetcode](https://leetcode.com/problems/product-of-array-except-self/)

Given an integer array `nums`, return _an array_ `answer` _such that_ `answer[i]` _is equal to the product of all the elements of_ `nums` _except_ `nums[i]`.

The product of any prefix or suffix of `nums` is **guaranteed** to fit in a **32-bit** integer.

You must write an algorithm that runs in `O(n)` time and without using the division operation.

**Example 1:**
```
Input: nums = [1,2,3,4]  
Output: [24,12,8,6]
```
**Example 2:**
```
Input: nums = [-1,1,0,-3,3]  
Output: [0,0,9,0,0]
```
**Constraints:**

-   `2 <= nums.length <= 105`
-   `-30 <= nums[i] <= 30`
-   The product of any prefix or suffix of `nums` is **guaranteed** to fit in a **32-bit** integer.

**Follow up:** Can you solve the problem in `O(1) `extra space complexity? (The output array **does not** count as extra space for space complexity analysis.)

---

```java
//Java  
class Solution {  
    public int[] productExceptSelf(int[] nums) {  
        int n = nums.length;  
        int[] ret = new int[n];  
        ret[0] = 1;  
        for (int i = 1; i < n; i++) {  
            ret[i] = ret[i - 1] * nums[i - 1];  
        }  
        int right = 1;  
        for (int i = n - 1; i >= 0; i--) {  
            ret[i] *= right;  
            right *= nums[i];  
        }  
        return ret;  
    }  
}
```

```csharp
//C#  
public class Solution {  
    public int[] ProductExceptSelf(int[] nums) {  
        int n = nums.Length;  
        int[] ret = new int[n];  
        ret[0] = 1;  
        for (int i = 1; i < n; i++) {  
            ret[i] = ret[i - 1] * nums[i - 1];  
        }  
        int right = 1;  
        for (int i = n - 1; i >= 0; i--) {  
            ret[i] *= right;  
            right *= nums[i];  
        }  
        return ret;  
    }  
}
```

---

Given numbers `[2, 3, 4, 5]`, regarding the third number 4, the product of array except 4 is `2*3*5` which consists of two parts: left `2*3` and right `5`. The product is `left*right`. We can get lefts and rights:
```
Numbers:     2    3    4     5  
Lefts:            2  2*3 2*3*4  
Rights:  3*4*5  4*5    5
```
Let’s fill the empty with 1:
```
Numbers:     2    3    4     5  
Lefts:       1    2  2*3 2*3*4  
Rights:  3*4*5  4*5    5     1
```
We can calculate lefts and rights in 2 loops. The time complexity is O(n).

We store lefts in result array. If we allocate a new array for rights. The space complexity is O(n). To make it O(1), we just need to store it in a variable which is `right` in @lycjava3’s code.




###### tags: `Leetcode` `Others`
