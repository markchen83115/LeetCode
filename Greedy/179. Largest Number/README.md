# Leetcode - 179. Largest Number (H-)

[Leetcode](https://leetcode.com/problemset/all/?search=179&page=1)

Given a list of non-negative integers `nums`, arrange them such that they form the largest number and return it.

Since the result may be very large, so you need to return a string instead of an integer.

**Example 1:**
```
Input: nums = [10,2]  
Output: "210"
```
**Example 2:**
```
Input: nums = [3,30,34,5,9]  
Output: "9534330"
```
**Constraints:**

-   `1 <= nums.length <= 100`
-   `0 <= nums[i] <= 109`

---

```java
// Java  
class Solution {  
    public String largestNumber(int[] nums) {  
        String[] numsStr = new String[nums.length];  
        for (int i = 0; i < nums.length; i++) {  
            numsStr[i] = "" + nums[i];  
        }  
        // '3', '30' => 330 compare 303  
        Arrays.sort(numsStr, (String a, String b) -> (b + a).compareTo(a + b));  
        if (numsStr[0].charAt(0) == '0') {  
            return "0";  
        }  
  
        StringBuffer sb  = new StringBuffer();  
        for (int i = 0; i < numsStr.length; i++) {  
            sb.append(numsStr[i]);  
        }  
        return sb.toString();     
    }  
}
```

Compare 2 numbers  
`a + b` compare with `b + a`, if `a + b > b + a` then sort as `a, b`  
ex: `30, 3` => compare `303 and 330` which is larger  
=> after sorting become`3, 30  
`answer is concat all sorted array


###### tags: `Leetcode` `Greedy` `Sort`