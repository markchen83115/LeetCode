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

參考 [LeetCode/Tree/105.Construct-Binary-Tree-from-Preorder-and-Inorder-Traversal at master · wisdompeak/LeetCode ( github.com )](https://github.com/wisdompeak/LeetCode/tree/master/Tree/105.Construct-Binary-Tree-from-Preorder-and-Inorder-Traversal)

本題是經典的二元樹操作。考慮到preorder的第一個元素必定是root；所以在inorder中找到對應root的位置後，其左邊就是左子樹的所有節點的中序遍歷，右邊就是右子樹的所有節點的中序遍歷。

那麼如何在preorder序列裡面區分出哪些是左子樹的節點，哪些是右子樹的節點呢？透過inorder序列裡左子樹節點的數目！假設inorder序列裡左子樹節點的數目為N，那麼在preorder序列裡，root之後的N個元素就是左子樹的先序遍歷，剩下的元素就是右子樹的先序遍歷。

注意，需要事先依照inorder建立Hash表，這樣從preorder確定root後，就可以立即查找到其在inorder裡的位置。

###### tags: `Leetcode` `Tree` `Tree & Sequence`