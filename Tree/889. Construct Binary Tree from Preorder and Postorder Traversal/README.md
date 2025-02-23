# Leetcode - 889. Construct Binary Tree from Preorder and Postorder Traversal (M+)

[Leetcode](https://leetcode.com/problems/construct-binary-tree-from-preorder-and-postorder-traversal/)

Given two integer arrays, `preorder` and `postorder` where `preorder` is the preorder traversal of a binary tree of **distinct** values and `postorder` is the postorder traversal of the same tree, reconstruct and return _the binary tree_.

If there exist multiple answers, you can **return any** of them.

**Example 1:**

> ![](https://assets.leetcode.com/uploads/2021/07/24/lc-prepost.jpg)
> 
> **Input:** preorder = [1,2,4,5,3,6,7], postorder = [4,5,2,6,7,3,1]
> **Output:** [1,2,3,4,5,6,7]

**Example 2:**

> **Input:** preorder = [1], postorder = [1]
> **Output:** [1]

**Constraints:**

-   `1 <= preorder.length <= 30`
-   `1 <= preorder[i] <= preorder.length`
-   All the values of `preorder` are **unique**.
-   `postorder.length == preorder.length`
-   `1 <= postorder[i] <= postorder.length`
-   All the values of `postorder` are **unique**.
-   It is guaranteed that `preorder` and `postorder` are the preorder traversal and postorder traversal of the same binary tree.

---
```java
// Java 2ms(Beats 7.59%), Time O(N), Space O(N)
class Solution {
    int preindex = 0;
    HashMap<Integer, Integer> postMap = new HashMap<>();
    public TreeNode constructFromPrePost(int[] pre, int[] post) {
        for(int i = 0; i < post.length; i++)
            postMap.put(post[i], i);
        return helper(pre, 0, post.length - 1);
    }

    TreeNode helper(int[] pre, int left, int right) {
        if(left > right)
            return null;
        
        TreeNode node = new TreeNode(pre[preindex++]);
        if(left == right)
            return node;
        
        int mid = postMap.get(pre[preindex]);
        node.left = helper(pre, left, mid);
        node.right = helper(pre, mid + 1, right - 1);
        return node;
    }
}
```
---


###### tags: `Leetcode` `Tree` `Tree & Sequence`