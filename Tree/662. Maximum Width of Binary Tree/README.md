# Leetcode - 662. Maximum Width of Binary Tree (H-)

[Leetcode](https://leetcode.com/problems/maximum-width-of-binary-tree/description/)

Given the `root` of a binary tree, return _the **maximum width** of the given tree_.

The **maximum width** of a tree is the maximum **width** among all levels.

The **width** of one level is defined as the length between the end-nodes (the leftmost and rightmost non-null nodes), where the null nodes between the end-nodes that would be present in a complete binary tree extending down to that level are also counted into the length calculation.

It is **guaranteed** that the answer will in the range of a **32-bit** signed integer.

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/05/03/width1-tree.jpg)

Input: root = [1,3,2,5,3,null,9]
Output: 4
Explanation: The maximum width exists in the third level with length 4 (5,3,null,9).

**Example 2:**

![](https://assets.leetcode.com/uploads/2022/03/14/maximum-width-of-binary-tree-v3.jpg)

Input: root = [1,3,2,5,null,null,9,6,null,7]
Output: 7
Explanation: The maximum width exists in the fourth level with length 7 (6,null,null,null,null,null,7).

**Example 3:**

![](https://assets.leetcode.com/uploads/2021/05/03/width3-tree.jpg)

Input: root = [1,3,2,5]
Output: 2
Explanation: The maximum width exists in the second level with length 2 (3,2).

**Constraints:**

-   The number of nodes in the tree is in the range `[1, 3000]`.
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
    public int widthOfBinaryTree(TreeNode root) {
        Deque<TreeNode> deque = new ArrayDeque<>();
        int ret = 0;
        root.val = 0;
        deque.offer(root);
        while (!deque.isEmpty()) {
            int size = deque.size();
            ret = Math.max(ret, deque.peekLast().val - deque.peekFirst().val + 1);
            for (int i = 0; i < size; i++) {
                TreeNode curr = deque.poll();
                if (curr.left != null) {
                    curr.left.val = curr.val * 2;
                    deque.offer(curr.left);
                }
                if (curr.right != null) {
                    curr.right.val = curr.val * 2 + 1;
                    deque.offer(curr.right);
                }
            }
        }

        return ret;
    }
}
```
---

```
        0
      0  1
    0 1 2 3

```
將`root = i`,
則`left node = i * 2`, `right node = i * 2 + 1`,
因此答案每個level的第一個node值 - 最後一個node值

透過Deque可以直接取得頭尾node, 
若用queue則需遍歷所有node


###### tags: `Leetcode` `Tree`
