# Leetcode - 110. Balanced Binary Tree (M+)

[Leetcode](https://leetcode.com/problems/balanced-binary-tree/description/)

Given a binary tree, determine if it is **height-balanced**.

A **height-balanced** binary tree is a binary tree in which the depth of the two subtrees of every node never differs by more than one.


**Example 1:**

![](https://assets.leetcode.com/uploads/2020/10/06/balance_1.jpg)
```
Input: root = [3,9,20,null,null,15,7]
Output: true
```
**Example 2:**

![](https://assets.leetcode.com/uploads/2020/10/06/balance_2.jpg)
```
Input: root = [1,2,2,3,3,null,null,4,4]
Output: false
```
**Example 3:**
```
Input: root = []
Output: true
```
**Constraints:**

-   The number of nodes in the tree is in the range `[0, 5000]`.
-   `-104 <= Node.val <= 104`

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
    public boolean isBalanced(TreeNode root) {
        if (depth(root) == -1) return false;
        return true;
    }

    public int depth(TreeNode root) {
        if (root == null) return 0;
        int left = depth(root.left);
        int right = depth(root.right);
        if (left == -1 || right == -1 || Math.abs(left - right) > 1) return -1;
        return 1 + Math.max(left, right);
    }
}
```
---

將題目`isBalanced()`轉為`depth()`
若計算depth中, 發現有`depth(root.left)`與`depth(root.right)`不合條件,
或是其中一個為-1, 則`depth()`提前`return -1`


###### tags: `Leetcode` `Tree`
