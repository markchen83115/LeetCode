# Leetcode - 2191. Sort the Jumbled Numbers (M)

[Leetcode](https://leetcode.com/problems/sort-the-jumbled-numbers/)

You are given a **0-indexed** integer array `mapping` which represents the mapping rule of a shuffled decimal system. `mapping[i] = j` means digit `i` should be mapped to digit `j` in this system.

The **mapped value** of an integer is the new integer obtained by replacing each occurrence of digit `i` in the integer with `mapping[i]` for all `0 <= i <= 9`.

You are also given another integer array `nums`. Return _the array _`nums`_ sorted in **non-decreasing** order based on the **mapped values** of its elements._

**Notes:**

-   Elements with the same mapped values should appear in the **same relative order** as in the input.
-   The elements of `nums` should only be sorted based on their mapped values and **not be replaced** by them.

**Example 1:**
```
Input: mapping = [8,9,4,0,2,1,3,5,7,6], nums = [991,338,38]
Output: [338,38,991]
Explanation: 
Map the number 991 as follows:
1. mapping[9] = 6, so all occurrences of the digit 9 will become 6.
2. mapping[1] = 9, so all occurrences of the digit 1 will become 9.
Therefore, the mapped value of 991 is 669.
338 maps to 007, or 7 after removing the leading zeros.
38 maps to 07, which is also 7 after removing leading zeros.
Since 338 and 38 share the same mapped value, they should remain in the same relative order, so 338 comes before 38.
Thus, the sorted array is [338,38,991].
```
**Example 2:**
```
Input: mapping = [0,1,2,3,4,5,6,7,8,9], nums = [789,456,123]
Output: [123,456,789]
Explanation: 789 maps to 789, 456 maps to 456, and 123 maps to 123. Thus, the sorted array is [123,456,789].
```
**Constraints:**

-   `mapping.length == 10`
-   `0 <= mapping[i] <= 9`
-   All the values of `mapping[i]` are **unique**.
-   `1 <= nums.length <= 3 * 104`
-   `0 <= nums[i] < 109`

---
```java
// Java 80ms(Beats 90.99%), Time O(N^2), Space O(N)
class Solution {
    public int[] sortJumbled(int[] mapping, int[] nums) {
        List<int[]> list = new ArrayList<>();
        
        for (int i = 0; i < nums.length; i++)
        {
            int num = nums[i];
            if (num == 0)
            {
                list.add(new int[] {mapping[0], i});
                continue;
            }
            
            int place = 1;
            int newNum = 0;
            while (num != 0)
            {
                newNum += mapping[num % 10] * place;
                place *= 10;
                num /= 10;
            }
            list.add(new int[] {newNum, i});
        }

        Collections.sort(list, (a, b) -> a[0] - b[0]);

        int[] ret = new int[nums.length];
        for (int i = 0; i < list.size(); i++)
            ret[i] = nums[list.get(i)[1]];

        return ret;
    }
}
```

```java
// Java 194ms(Beats 55.86%)
class Solution {
    public int[] sortJumbled(int[] mapping, int[] nums) {
        List<Integer> list = new ArrayList<>();
        List<Integer> newList = new ArrayList<>();
        
        for (int i = 0; i < nums.length; i++)
            list.add(i);

        for (int num : nums)
        {
            StringBuilder sb = new StringBuilder();
            for (char ch : (""+num).toCharArray())
                sb.append(mapping[ch - '0']);
            
            newList.add(Integer.valueOf(sb.toString()));
        }

        Collections.sort(list, (a, b) -> newList.get(a) - newList.get(b));

        int[] ret = new int[nums.length];
        for (int i = 0; i < list.size(); i++)
            ret[i] = nums[list.get(i)];

        return ret;
    }
}
```
---

將每個數字轉成String再轉為char array,
將每個digit替換為新的digit並組成新數字
再以新數字進行排序

###### tags: `Leetcode` `Math`