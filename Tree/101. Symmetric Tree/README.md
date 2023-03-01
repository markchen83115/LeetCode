# Leetcode - 101. Symmetric Tree (E)

[Leetcode](https://leetcode.com/problems/symmetric-tree/description/)

Given the `root` of a binary tree, _check whether it is a mirror of itself_ (i.e., symmetric around its center).

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/02/19/symtree1.jpg)
```
Input: root = [1,2,2,3,4,4,3]
Output: true
```
**Example 2:**

![](https://assets.leetcode.com/uploads/2021/02/19/symtree2.jpg)
```
Input: root = [1,2,2,null,3,null,3]
Output: false
```
**Constraints:**

-   The number of nodes in the tree is in the range `[1, 1000]`.
-   `-100 <= Node.val <= 100`

**Follow up:** Could you solve it both recursively and iteratively?

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
// Java Recursive 0ms
class Solution {
    public boolean isSymmetric(TreeNode root) {
        if (root == null) return true;
        return dfs(root.left, root.right);
    }

    public boolean dfs(TreeNode left, TreeNode right) {
        if (left == null || right == null)
            return left == right;
        if (left.val != right.val)
            return false;
        return dfs(left.left, right.right) && dfs(left.right, right.left);
    }
}
```

```java
// Java Iteration 1ms
class Solution {
    public boolean isSymmetric(TreeNode root) {
        if (root == null) return true;
        Queue<TreeNode> queue = new ArrayDeque<>();
        if (root.left != null) 
            queue.offer(root.left);
        if (root.right != null) 
            queue.offer(root.right);

        while (!queue.isEmpty()) {
            int size = queue.size();
            if (size % 2 != 0) return false;
            for (int i = 0; i < size / 2; i++) {
                TreeNode leftNode = queue.poll();
                TreeNode rightNode = queue.poll();
                if (leftNode.val != rightNode.val) return false;
                if (leftNode.left != null && rightNode.right != null) {
                    queue.offer(leftNode.left);
                    queue.offer(rightNode.right);
                } else if (leftNode.left != null || rightNode.right != null) {
                    return false;
                }
                if (leftNode.right != null && rightNode.left != null) {
                    queue.offer(leftNode.right);
                    queue.offer(rightNode.left);
                } else if (leftNode.right != null || rightNode.left != null) {
                    return false;
                }
                    
            }
        }
        return true;
    }
}
```

```java
// Java [Stack can handle null]
class Solution {
    public boolean isSymmetric(TreeNode root) {
        if (root == null) return true;
        Stack<TreeNode> stack = new Stack<>();
        stack.push(root.left);
        stack.push(root.right);
        while (!stack.empty()) {
            TreeNode left = stack.pop(), right = stack.pop();
            if (left == null && right == null) continue;
            if (left == null || right == null || left.val != right.val) return false;
            stack.push(left.left);
            stack.push(right.right);
            stack.push(left.right);
            stack.push(right.left);
        }
        return true;
    }
}
```



---

###### tags: `Leetcode` `Tree`
