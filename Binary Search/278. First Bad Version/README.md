# Leetcode - 278. First Bad Version (E)

[Leetcode](https://leetcode.com/problems/first-bad-version/)

You are a product manager and currently leading a team to develop a new product. Unfortunately, the latest version of your product fails the quality check. Since each version is developed based on the previous version, all the versions after a bad version are also bad.

Suppose you have `n` versions `[1, 2, ..., n]` and you want to find out the first bad one, which causes all the following ones to be bad.

You are given an API `bool isBadVersion(version)` which returns whether `version` is bad. Implement a function to find the first bad version. You should minimize the number of calls to the API.

**Example 1:**
```
Input: n = 5, bad = 4  
Output: 4  
Explanation:  
call isBadVersion(3) -> false  
call isBadVersion(5) -> true  
call isBadVersion(4) -> true  
Then 4 is the first bad version.
```
**Example 2:**
```
Input: n = 1, bad = 1  
Output: 1
```
**Constraints:**

-   `1 <= bad <= n <= 231 - 1`

---

```java
// Java  
// FFFTTTTT => find leftmost T  
  
/* The isBadVersion API is defined in the parent class VersionControl.  
      boolean isBadVersion(int version); */  
public class Solution extends VersionControl {  
    public int firstBadVersion(int n) {  
        int low = 1, high = n;  
        //FFFTTTTT  
        while (low < high) {  
            int mid = low + (high - low) / 2;  
            if (isBadVersion(mid)) {  
                high = mid;  
            } else {  
                low = mid + 1;  
            }  
        }  
        return low;  
    }  
}
```

```java
// Java  
// FFFTTTTT => find rightmost F  
  
/* The isBadVersion API is defined in the parent class VersionControl.  
      boolean isBadVersion(int version); */  
public class Solution extends VersionControl {  
    public int firstBadVersion(int n) {  
        int low = 1, high = n;  
        //FFFTTTTT  
        while (low < high) {  
            int mid = low + (high - low) / 2 + 1;  
            if (isBadVersion(mid)) {  
                high = mid - 1;  
            } else {  
                low = mid;  
            }  
        }  
        // return the (rightmost F) + 1  
        return low + 1;  
    }  
}

```
---


1.  Check `low < high` will not be infinite cycle.
2.  When finding leftmost T / rightmost F,  
    use last `FT` or `01` condition to check the return value.


###### tags: `Leetcode` `Binary Search`
