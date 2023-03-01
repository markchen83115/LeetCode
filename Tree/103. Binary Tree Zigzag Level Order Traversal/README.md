# Leetcode - 103. Binary Tree Zigzag Level Order Traversal (M-)

[Leetcode](https://leetcode.com/problems/binary-tree-zigzag-level-order-traversal/description/)

Given the `root` of a binary tree, return _the zigzag level order traversal of its nodes' values_. (i.e., from left to right, then right to left for the next level and alternate between).

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/02/19/tree1.jpg)

Input: root = [3,9,20,null,null,15,7]
Output: [[3],[20,9],[15,7]]

**Example 2:**

Input: root = [1]
Output: [[1]]

**Example 3:**

Input: root = []
Output: []

**Constraints:**

-   The number of nodes in the tree is in the range `[0, 2000]`.
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
    public List<List<Integer>> zigzagLevelOrder(TreeNode root) {
        List<List<Integer>> ret = new ArrayList<>();
        if (root == null) return ret;
        Queue<TreeNode> queue = new LinkedList<>();
        boolean zigzag = true;
        queue.offer(root);

        while (!queue.isEmpty()) {
            int size = queue.size();
            List<Integer> list = new LinkedList<>();
            for (int i = 0; i < size; i++) {
                TreeNode currNode = queue.poll();
                if (zigzag) {
                    list.add(currNode.val);
                } else {
                    list.add(0, currNode.val);
                }
                if (currNode.left != null) 
                    queue.offer(currNode.left);
                if (currNode.right != null) 
                    queue.offer(currNode.right);
            }
            ret.add(new ArrayList(list));
            zigzag = !zigzag;
        }
        return ret;
    }
}
```
---

使用List.add(0, value)將元素放進index 0的位置達到reverse的效果
因為都在頭尾add元素, 因此List使用LinkedList來實作速度較快
```javascript
Algorithm           ArrayList   LinkedList
seek front            O(1)         O(1)
seek back             O(1)         O(1)
seek to index         O(1)         O(N)
insert at front       O(N)         O(1)
insert at back        O(1)         O(1)
insert after an item  O(N)         O(1)
```


###### tags: `Leetcode` `Tree`
