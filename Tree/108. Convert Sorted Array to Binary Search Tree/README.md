# Leetcode - 108. Convert Sorted Array to Binary Search Tree (E)

[Leetcode](https://leetcode.com/problems/convert-sorted-array-to-binary-search-tree/description/)

Given an integer array `nums` where the elements are sorted in **ascending order**, convert _it to a _ 

**_height-balanced_**

 _binary search tree_.

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/02/18/btree1.jpg)
```
Input: nums = [-10,-3,0,5,9]
Output: [0,-3,9,-10,null,5]
Explanation: [0,-10,5,null,-3,null,9] is also accepted:
```
![](https://assets.leetcode.com/uploads/2021/02/18/btree2.jpg)

**Example 2:**

![](https://assets.leetcode.com/uploads/2021/02/18/btree.jpg)
```
Input: nums = [1,3]
Output: [3,1]
Explanation: [1,null,3] and [3,1] are both height-balanced BSTs.
```
**Constraints:**

-   `1 <= nums.length <= 104`
-   `-104 <= nums[i] <= 104`
-   `nums` is sorted in a **strictly increasing** order.

---

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode() {}
 *     TreeNode(int val) { this.val = val; }
 *     TreeNode(int val, TreeNode left, TreeNode right) {
 *         this.val = val;
 *         this.left = left;
 *         this.right = right;
 *     }
 * }
 */
// Java
class Solution {
    public TreeNode sortedArrayToBST(int[] nums) {
        return helper(nums, 0, nums.length - 1);
    }
    
    public TreeNode helper(int[] nums, int left, int right) {
        if (left <= right) {
            int mid = left + (right - left) / 2;
            TreeNode root = new TreeNode(nums[mid]);
            root.left = helper(nums, left, mid - 1);
            root.right = helper(nums, mid + 1, right);
            return root;
        }
        return null;
    }
}
```

---

利用binary search 將最中間的點抓出做為新的node
左葉子節點為(left, mid - 1)的中間點
右葉子節點為(mid + 1, right)的中間點


###### tags: `Leetcode` `Tree`
