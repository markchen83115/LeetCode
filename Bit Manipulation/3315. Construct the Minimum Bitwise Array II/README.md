# Leetcode - 3315. Construct the Minimum Bitwise Array II (M)

[Leetcode](https://leetcode.com/problems/construct-the-minimum-bitwise-array-ii/)

You are given an array `nums` consisting of `n`  

prime

 integers.

You need to construct an array `ans` of length `n`, such that, for each index `i`, the bitwise `OR` of `ans[i]` and `ans[i] + 1` is equal to `nums[i]`, i.e. `ans[i] OR (ans[i] + 1) == nums[i]`.

Additionally, you must **minimize** each value of `ans[i]` in the resulting array.

If it is _not possible_ to find such a value for `ans[i]` that satisfies the **condition**, then set `ans[i] = -1`.

**Example 1:**
```
Input: nums = [2,3,5,7]
Output: [-1,1,4,3]
Explanation:
-   For `i = 0`, as there is no value for `ans[0]` that satisfies `ans[0] OR (ans[0] + 1) = 2`, so `ans[0] = -1`.
-   For `i = 1`, the smallest `ans[1]` that satisfies `ans[1] OR (ans[1] + 1) = 3` is `1`, because `1 OR (1 + 1) = 3`.
-   For `i = 2`, the smallest `ans[2]` that satisfies `ans[2] OR (ans[2] + 1) = 5` is `4`, because `4 OR (4 + 1) = 5`.
-   For `i = 3`, the smallest `ans[3]` that satisfies `ans[3] OR (ans[3] + 1) = 7` is `3`, because `3 OR (3 + 1) = 7`.
```
**Example 2:**
```
Input: nums = [11,13,31]
Output: [9,12,15]
Explanation:
-   For `i = 0`, the smallest `ans[0]` that satisfies `ans[0] OR (ans[0] + 1) = 11` is `9`, because `9 OR (9 + 1) = 11`.
-   For `i = 1`, the smallest `ans[1]` that satisfies `ans[1] OR (ans[1] + 1) = 13` is `12`, because `12 OR (12 + 1) = 13`.
-   For `i = 2`, the smallest `ans[2]` that satisfies `ans[2] OR (ans[2] + 1) = 31` is `15`, because `15 OR (15 + 1) = 31`.
```
**Constraints:**

-   `1 <= nums.length <= 100`
-   `2 <= nums[i] <= 109`
-   `nums[i]` is a prime number.

---
```java
// Java 1ms(Beats 100.00%), Time O(N), Space O(N)
class Solution {
    public int[] minBitwiseArray(List<Integer> A) {
        int n = A.size(), res[] = new int[n];
        for (int i = 0; i < n; i++) {
            int a = A.get(i);
            if (A.get(i) % 2 == 0) {
                res[i] = -1;
            } else {
                res[i] = a - ((a + 1) & (-a - 1)) / 2;
            }
        }
        return res;
    }
}
```

```java
// Java MyAnswer 6ms(Beats 13.40%), Time O(NlogK), Space O(N)
class Solution {
    public int[] minBitwiseArray(List<Integer> nums) {
        int n = nums.size();
        int[] ret = new int[n];
        for (int i = 0; i < n; i++)
        {
            int k = nums.get(i);
            if (k == 2)
            {
                ret[i] = -1;
                continue;
            }
                
            char[] s = Integer.toBinaryString(k).toCharArray();
            for (int j = s.length - 1; j >= 0; j--)
            {
                if (s[j] == '1')
                {
                    s[j] = '0';
                    if (j + 1 < s.length)
                        s[j+1] = '1';
                }
                else
                    break;
            }

            ret[i] = Integer.valueOf(new String(s), 2);
        }

        return ret;
    }
}
```
---
[[Java/C++/Python] Bit, O(n)](https://leetcode.com/problems/construct-the-minimum-bitwise-array-ii/solutions/5904140/java-c-python-bit-o-n)
`..0111 || (..0111 + 1) = ..1111`  
The effect is that change the rightmost 0 to 1.

For each `a` in array `A`,  
find the rightmost 0,  
and set the next bit from 1 to 0.

`a + 1` will filp the 1-suffix to 0-suffix,  
and flip the rightmost 0 to 1.  
`a & -a` is a trick to get the rightmost 1.
![image](https://hackmd.io/_uploads/SJuQpHp1ye.png)


###### tags: `Leetcode` `Bit Manipulation`
