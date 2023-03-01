# Leetcode - 102. Binary Tree Level Order Traversal (M-)

[Leetcode](https://leetcode.com/problems/binary-tree-level-order-traversal/description/)

Given the `root` of a binary tree, return _the level order traversal of its nodes' values_. (i.e., from left to right, level by level).

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/02/19/tree1.jpg)
```
Input: root = [3,9,20,null,null,15,7]
Output: [[3],[9,20],[15,7]]
```
**Example 2:**
```
Input: root = [1]
Output: [[1]]
```
**Example 3:**
```
Input: root = []
Output: []
```
**Constraints:**

-   The number of nodes in the tree is in the range `[0, 2000]`.
-   `-1000 <= Node.val <= 1000`

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
    public List<List<Integer>> levelOrder(TreeNode root) {
        List<List<Integer>> ret = new ArrayList<>();
        if (root == null)
            return ret;
        
        Queue<TreeNode> queue = new LinkedList<>();
        queue.offer(root);

        while (!queue.isEmpty()) {
            int size = queue.size();
            List<Integer> list = new ArrayList<>();
            for (int i = 0; i < size; i++) {
                TreeNode currNode = queue.poll();
                if (currNode.left != null)
                    queue.offer(currNode.left);
                if (currNode.right != null)
                    queue.offer(currNode.right);
                list.add(currNode.val);
            }
            ret.add(list);
        }
        return ret;
    }
}
```
---

透過queue將各層的TreeNode放進去,
利用size去紀錄這個level有幾個TreeNode


###### tags: `Leetcode` `Graph`
