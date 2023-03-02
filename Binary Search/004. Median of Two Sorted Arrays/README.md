# Leetcode - 4. Median of Two Sorted Arrays(H)

[Leetcode](https://leetcode.com/problems/median-of-two-sorted-arrays/description/)

Given two sorted arrays `nums1` and `nums2` of size `m` and `n` respectively, return **the median** of the two sorted arrays.

The overall run time complexity should be `O(log (m+n))`.

**Example 1:**
```
Input: nums1 = [1,3], nums2 = [2]
Output: 2.00000
Explanation: merged array = [1,2,3] and median is 2.
```
**Example 2:**
```
Input: nums1 = [1,2], nums2 = [3,4]
Output: 2.50000
Explanation: merged array = [1,2,3,4] and median is (2 + 3) / 2 = 2.5.
```
**Constraints:**

-   `nums1.length == m`
-   `nums2.length == n`
-   `0 <= m <= 1000`
-   `0 <= n <= 1000`
-   `1 <= m + n <= 2000`
-   `-106 <= nums1[i], nums2[i] <= 106`

---

```java
class Solution {
    public double findMedianSortedArrays(int[] nums1, int[] nums2) {
        int m = nums1.length;
        int n = nums2.length;
        if ((m + n) % 2 == 1) {
            return findKthElement(nums1, 0, nums2, 0, (m+n)/2 + 1);
        } else {
            return (findKthElement(nums1, 0, nums2, 0, (m+n)/2) + findKthElement(nums1, 0, nums2, 0, (m+n)/2 + 1)) * 1.0 / 2;
        }
    }

    public double findKthElement(int[] nums1, int i, int[] nums2, int j, int k) {
        int m = nums1.length;
        int n = nums2.length;
        int k1, k2; // 取的個數

        // 短的放前面
        if (m - i > n - j)
            return findKthElement(nums2, j, nums1, i, k);

        // 若m已被取完, 則直接取n的數組
        if (i == m) {
            return nums2[j + k - 1];
        }

        //  只取1個, 則取小的元素
        if (k == 1) {
            return Math.min(nums1[i], nums2[j]);
        }

        // 當m被取k/2個時，會全被取完
        if (i + k/2 - 1 >= m - 1) {  // index角度
            k1 = m - i;
        } else {
            k1 = k/2;
        }
        k2 = k - k1;

        if (nums1[i + k1 - 1] < nums2[j + k2 - 1]) {    // X0 < Y0 去除X一半 
            return findKthElement(nums1, i + k1, nums2, j, k - k1);
        } else {
            return findKthElement(nums1, i, nums2, j + k2, k - k2);
        }

        
    }
}
```

---

兩個數列, 若各取k/2個(總和k)數字, 來尋找第k大的數字  
```
XXX[X0] XX
YYY[Y0] YYYY
```

若X0 < Y0 => 刪除`XXX[X0]`, 因為X0不可能為第k大的數字  
同理  
若X0 > Y0 => 刪除`YYY[Y0]`, 因為Y0不可能為第k大的數字  



###### tags: `Leetcode` `Binary Search`
