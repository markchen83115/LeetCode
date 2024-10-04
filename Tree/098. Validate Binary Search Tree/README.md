# Leetcode - 98. Validate Binary Search Tree (M)

[Leetcode](https://leetcode.com/problems/validate-binary-search-tree/description/)

Given the `root` of a binary tree, _determine if it is a valid binary search tree (BST)_.

A **valid BST** is defined as follows:

-   The left 
    
    subtree
    
     of a node contains only nodes with keys **less than** the node's key.
-   The right subtree of a node contains only nodes with keys **greater than** the node's key.
-   Both the left and right subtrees must also be binary search trees.

**Example 1:**

![](https://assets.leetcode.com/uploads/2020/12/01/tree1.jpg)
```
Input: root = [2,1,3]
Output: true
```
**Example 2:**

![](https://assets.leetcode.com/uploads/2020/12/01/tree2.jpg)
```
Input: root = [5,1,4,null,null,3,6]
Output: false
Explanation: The root node's value is 5 but its right child's value is 4.
```
**Constraints:**

-   The number of nodes in the tree is in the range `[1, 104]`.
-   `-231 <= Node.val <= 231 - 1`

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
// Java Inorder - Recursive
class Solution {
    TreeNode last;
    public boolean isValidBST(TreeNode root) {
        if (root == null)
            return true;
        //L
        if (!isValidBST(root.left))
            return false;
        //V
        if (last != null && root.val <= last.val)
            return false;
        last = root; 
        //R
        return isValidBST(root.right);
    }
}
```

```java
// Java Inorder - Stack
class Solution {
    public boolean isValidBST(TreeNode root) {
        if (root == null)
            return true;
        TreeNode last = null;
        Stack<TreeNode> stack = new Stack<>();
        while (root != null || !stack.isEmpty()) {
            //L
            while (root != null) {
                stack.push(root);
                root = root.left;
            }
            //V
            root = stack.pop();
            if (last != null && root.val <= last.val)
                return false;
            last = root;
            //R
            root = root.right;
        }
        return true;
    }
}
```

---

使用Inorder來從小到大遍例所有node
若有`node <= 前一個node`, 代表不是Binary Search Tree

可用recursive or stack來處理inorder

---
* BST 延伸閱讀

[Learn one iterative inorder traversal, apply it to multiple tree questions (Java Solution)](https://leetcode.com/problems/validate-binary-search-tree/solutions/32112/learn-one-iterative-inorder-traversal-apply-it-to-multiple-tree-questions-java-solution)

I will show you all how to tackle various tree questions using iterative inorder traversal. First one is the standard iterative inorder traversal using stack. Hope everyone agrees with this solution.

Question : [Binary Tree Inorder Traversal](https://leetcode.com/problems/binary-tree-inorder-traversal/)

```java
public List<Integer> inorderTraversal(TreeNode root) {
    List<Integer> list = new ArrayList<>();
    if(root == null) return list;
    Stack<TreeNode> stack = new Stack<>();
    while(root != null || !stack.empty()){
        while(root != null){
            stack.push(root);
            root = root.left;
        }
        root = stack.pop();
        list.add(root.val);
        root = root.right;
        
    }
    return list;
}
```

Now, we can use this structure to find the Kth smallest element in BST.

Question : [Kth Smallest Element in a BST](https://leetcode.com/problems/kth-smallest-element-in-a-bst/)

```java
 public int kthSmallest(TreeNode root, int k) {
     Stack<TreeNode> stack = new Stack<>();
     while(root != null || !stack.isEmpty()) {
         while(root != null) {
             stack.push(root);    
             root = root.left;   
         } 
         root = stack.pop();
         if(--k == 0) break;
         root = root.right;
     }
     return root.val;
 }
```

We can also use this structure to solve BST validation question.

Question : [Validate Binary Search Tree](https://leetcode.com/problems/validate-binary-search-tree/)

```java
public boolean isValidBST(TreeNode root) {
   if (root == null) return true;
   Stack<TreeNode> stack = new Stack<>();
   TreeNode pre = null;
   while (root != null || !stack.isEmpty()) {
      while (root != null) {
         stack.push(root);
         root = root.left;
      }
      root = stack.pop();
      if(pre != null && root.val <= pre.val) return false;
      pre = root;
      root = root.right;
   }
   return true;
}
```



###### tags: `Leetcode` `Tree`