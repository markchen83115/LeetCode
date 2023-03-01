# Leetcode - 104. Maximum Depth of Binary Tree (E)

[Leetcode](https://leetcode.com/problems/maximum-depth-of-binary-tree/description/)

Given the `root` of a binary tree, return _its maximum depth_.

A binary tree's **maximum depth** is the number of nodes along the longest path from the root node down to the farthest leaf node.

**Example 1:**

![](https://assets.leetcode.com/uploads/2020/11/26/tmp-tree.jpg)
```
Input: root = [3,9,20,null,null,15,7]
Output: 3
```
**Example 2:**
```
Input: root = [1,null,2]
Output: 2
```
**Constraints:**

-   The number of nodes in the tree is in the range `[0, 104]`.
-   `-100 <= Node.val <= 100`

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
    public int maxDepth(TreeNode root) {
        return dfs(root);
    }

    public int dfs(TreeNode root) {
        if (root == null) return 0;
        int left = dfs(root.left);
        int right = dfs(root.right);
        return Math.max(left, right) + 1;
    }
}
```


---

###### tags: `Leetcode` `Tree`
