# Leetcode - 199. Binary Tree Right Side View (M)

[Leetcode](https://leetcode.com/problems/binary-tree-right-side-view/description/)

Given the `root` of a binary tree, imagine yourself standing on the **right side** of it, return _the values of the nodes you can see ordered from top to bottom_.

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/02/14/tree.jpg)
```
Input: root = [1,2,3,null,5,null,4]
Output: [1,3,4]
```
**Example 2:**
```
Input: root = [1,null,3]
Output: [1,3]
```
**Example 3:**
```
Input: root = []
Output: []
```
**Constraints:**

-   The number of nodes in the tree is in the range `[0, 100]`.
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
// Java Iteration
class Solution {
    List<Integer> ret = new ArrayList<>();
    public List<Integer> rightSideView(TreeNode root) {
        viewRight(root, 0);
        return ret;
    }

    public void viewRight(TreeNode root, int level) {
        if (root == null)
            return;
        if (ret.size() == level)
            ret.add(root.val);

        viewRight(root.right, level + 1);
        viewRight(root.left,  level + 1);
    }
}
```

```java
// Java Queue
class Solution {
    public List<Integer> rightSideView(TreeNode root) {
        List<Integer> ret = new ArrayList<>();
        if (root == null)
            return ret;

        Queue<TreeNode> queue = new LinkedList<>();
        queue.offer(root);
        while (!queue.isEmpty()) {
            int size = queue.size();
            boolean getRight = false;
            for (int i = 0; i < size; i++) {
                TreeNode currNode = queue.poll();
                if (currNode.right != null)
                    queue.offer(currNode.right);
                if (currNode.left != null)
                    queue.offer(currNode.left);
                if (!getRight) {
                    ret.add(currNode.val);
                    getRight = true;
                }
            }
        }
        return ret;
    }
}
```

---

###### tags: `Leetcode` `Tree`
