# Leetcode - 106. Construct Binary Tree from Inorder and Postorder Traversal (M+)

[Leetcode](https://leetcode.com/problems/construct-binary-tree-from-inorder-and-postorder-traversal/)

Given two integer arrays `inorder` and `postorder` where `inorder` is the inorder traversal of a binary tree and `postorder` is the postorder traversal of the same tree, construct and return _the binary tree_.

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/02/19/tree.jpg)
```
Input: inorder = [9,3,15,20,7], postorder = [9,15,7,20,3]
Output: [3,9,20,null,null,15,7]
```
**Example 2:**
```
Input: inorder = [-1], postorder = [-1]
Output: [-1]
```
**Constraints:**

-   `1 <= inorder.length <= 3000`
-   `postorder.length == inorder.length`
-   `-3000 <= inorder[i], postorder[i] <= 3000`
-   `inorder` and `postorder` consist of **unique** values.
-   Each value of `postorder` also appears in `inorder`.
-   `inorder` is **guaranteed** to be the inorder traversal of the tree.
-   `postorder` is **guaranteed** to be the postorder traversal of the tree.

---
```java
// Java 2ms(Beats 63.29%), Time O(N), Space O(N)
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
class Solution {
    HashMap<Integer, Integer> inorderMap = new HashMap<>();
    int postIndex;
    public TreeNode buildTree(int[] inorder, int[] postorder) {
        this.postIndex = postorder.length - 1;
        for (int i = 0; i < inorder.length; i++)
            inorderMap.put(inorder[i], i);
        
        return helper(postorder, 0, inorder.length - 1);
    }

    TreeNode helper(int[] postorder, int left, int right)
    {
        if (left > right)
            return null;
        int val = postorder[postIndex--];
        TreeNode node = new TreeNode(val);                              // V
        node.right = helper(postorder, inorderMap.get(val) + 1, right); // R
        node.left = helper(postorder, left, inorderMap.get(val) - 1);   // L
        return node;
    }
}

// inorder = [1,9,3,15,20,7], postorder = [1,9,15,7,20,3]
//              L V R                        L      R. V. <-- V R L (反過來看)
```
---

有了[Leetcode - 105. Construct Binary Tree from Preorder and Inorder Traversal](https://hackmd.io/4hC6uXAVR7WBcwAeeWjI5g?view)的基礎

只需要將`postorder(LRV)`的順序反過來看, 即為`VRL`
因此從`postorder`的最後一個元素開始, 跑到第一個元素
作法與`105. Construct Binary Tree from Preorder and Inorder Traversal`相同



###### tags: `Leetcode` `Tree` `Tree & Sequence`