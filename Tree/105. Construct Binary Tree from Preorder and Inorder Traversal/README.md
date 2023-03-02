# Leetcode - 105. Construct Binary Tree from Preorder and Inorder Traversal (H-)

[Leetcode](https://leetcode.com/problems/construct-binary-tree-from-preorder-and-inorder-traversal/description/)

Given two integer arrays `preorder` and `inorder` where `preorder` is the preorder traversal of a binary tree and `inorder` is the inorder traversal of the same tree, construct and return _the binary tree_.

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/02/19/tree.jpg)
```
Input: preorder = [3,9,20,15,7], inorder = [9,3,15,20,7]
Output: [3,9,20,null,null,15,7]
```
**Example 2:**
```
Input: preorder = [-1], inorder = [-1]
Output: [-1]
```
**Constraints:**

-   `1 <= preorder.length <= 3000`
-   `inorder.length == preorder.length`
-   `-3000 <= preorder[i], inorder[i] <= 3000`
-   `preorder` and `inorder` consist of **unique** values.
-   Each value of `inorder` also appears in `preorder`.
-   `preorder` is **guaranteed** to be the preorder traversal of the tree.
-   `inorder` is **guaranteed** to be the inorder traversal of the tree.

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
    int preStart = 0;
    HashMap<Integer, Integer> inorderMap = new HashMap<>();
    public TreeNode buildTree(int[] preorder, int[] inorder) {
        for (int i = 0; i < inorder.length; i++)
            inorderMap.put(inorder[i], i);
        
        return helper(preorder, 0, preorder.length - 1);
    }

    public TreeNode helper(int[] preorder, int left, int right) {
        if (left > right) return null;

        int rootVal = preorder[preStart++];
        TreeNode root = new TreeNode(rootVal);
        root.left = helper(preorder, left, inorderMap.get(rootVal) - 1);
        root.right = helper(preorder, inorderMap.get(rootVal) + 1, right);
        return root;
    }

    // preorder = [3,9,1,20,15,7], inorder = [1,9,(3),15,20,7]
    //             V L   R                      L  V  R
}
```



---

###### tags: `Leetcode` `Tree` `Tree & Sequence`
