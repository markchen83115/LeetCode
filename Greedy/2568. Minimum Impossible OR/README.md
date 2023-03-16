# Leetcode - 2568. Minimum Impossible OR (H-)

[Leetcode](https://leetcode.com/problems/minimum-impossible-or/description/)

You are given a **0-indexed** integer array `nums`.

We say that an integer x is **expressible** from `nums` if there exist some integers `0 <= index1 < index2 < ... < indexk < nums.length` for which `nums[index1] | nums[index2] | ... | nums[indexk] = x`. In other words, an integer is expressible if it can be written as the bitwise OR of some subsequence of `nums`.

Return _the minimum **positive non-zero integer** that is not __expressible from _`nums`.

**Example 1:**
```
Input: nums = [2,1]
Output: 4
Explanation: 1 and 2 are already present in the array. We know that 3 is expressible, since nums[0] | nums[1] = 2 | 1 = 3. Since 4 is not expressible, we return 4.
```
**Example 2:**
```
Input: nums = [5,3,2]
Output: 1
Explanation: We can show that 1 is the smallest number that is not expressible.
```
**Constraints:**

-   `1 <= nums.length <= 105`
-   `1 <= nums[i] <= 109`

---
```java
// Java
class Solution {
    public int minImpossibleOR(int[] nums) {
        Arrays.sort(nums);
        int max = 0;
        for (int num : nums) {
            if (num > max + 1)
                return max + 1;
            max = (max | num);
        }
        return max + 1;
    }
}
```

```java
// Java
class Solution {
    public int minImpossibleOR(int[] nums) {
        HashSet<Integer> set = new HashSet<>();
        for (int num : nums) {
            set.add(num);
        }
        int ret = 1;
        for (int i = 0; i < 32; i++) {
            if (!set.contains(ret))
                return ret;
            ret <<= 1;
        }
        return -1;
    }
}
```
---
[wisdompeak/YouTube](https://www.youtube.com/watch?v=ROoz5NI6CIM)
> 解法1

此題與1798. Maximum Number of Consecutive Values You Can Make相同類型
假設從`[1 : num[n-1]]`可將`[1 : max]`的區間數值都組合出來
此時去檢查`if (num[i] > max + 1)`的話, 則不可能組合出`max + 1`
若`num[i] <= max + 1`的話, 則可以組合出`[1 : max+nums[i]]`的區間

> 解法2
以bit表示, 若目前有`1, 10, 100, 1000`
則可組合出`[0 : 1111]`的區間, 但無法組出`10000`
因此若下一個數大於`10000`, 
ex: 下一個數為`10001`則不可能組合出`10000`
因此只需要檢查是否有`1, 2, 4, 8, 16, ..`


###### tags: `Leetcode` `Greedy` `Constructive Problems`
