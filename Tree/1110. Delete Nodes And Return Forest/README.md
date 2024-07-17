# Leetcode - 1110. Delete Nodes And Return Forest (M)

[Leetcode](https://leetcode.com/problems/delete-nodes-and-return-forest/)

Given the `root` of a binary tree, each node in the tree has a distinct value.

After deleting all nodes with a value in `to_delete`, we are left with a forest (a disjoint union of trees).

Return the roots of the trees in the remaining forest. You may return the result in any order.

**Example 1:**

![](https://assets.leetcode.com/uploads/2019/07/01/screen-shot-2019-07-01-at-53836-pm.png)

Input: root = [1,2,3,4,5,6,7], to_delete = [3,5]
Output: [[1,2,null,4],[6],[7]]

**Example 2:**

Input: root = [1,2,4,null,3], to_delete = [3]
Output: [[1,2,4]]

**Constraints:**

-   The number of nodes in the given tree is at most `1000`.
-   Each node has a distinct value between `1` and `1000`.
-   `to_delete.length <= 1000`
-   `to_delete` contains distinct values between `1` and `1000`.

---
```java
// Java 1ms (Beats 97.28%), Time O(N), Space O(N)
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
    List<TreeNode> ret = new ArrayList<>();
    HashSet<Integer> del = new HashSet<>();
    public List<TreeNode> delNodes(TreeNode root, int[] to_delete) {
        for (int d : to_delete)
            del.add(d);
        
        if (dfs(root) != null)
            ret.add(root);
        return ret;
    }

    TreeNode dfs(TreeNode root)
    {
        if (root == null)
            return null;
        
        root.left = dfs(root.left);
        root.right = dfs(root.right);

        if (del.contains(root.val))
        {
            if (root.left != null)
                ret.add(root.left);
            if (root.right != null)
                ret.add(root.right);
            return null;
        }
        return root;
        
    }
}
```
---

透過遞迴往最底下走, 一路回傳子節點的狀況, 才能透過子節點判斷是否為新root

###### tags: `Leetcode` `Tree`