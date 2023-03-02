# Leetcode - 863. All Nodes Distance K in Binary Tree (H-)

[Leetcode](https://leetcode.com/problems/all-nodes-distance-k-in-binary-tree/description/)

Given the `root` of a binary tree, the value of a target node `target`, and an integer `k`, return _an array of the values of all nodes that have a distance _`k`_ from the target node._

You can return the answer in **any order**.

**Example 1:**

![](https://s3-lc-upload.s3.amazonaws.com/uploads/2018/06/28/sketch0.png)
```
Input: root = [3,5,1,6,2,0,8,null,null,7,4], target = 5, k = 2
Output: [7,4,1]
Explanation: The nodes that are a distance 2 from the target node (with value 5) have values 7, 4, and 1.
```
**Example 2:**
```
Input: root = [1], target = 1, k = 3
Output: []
```
**Constraints:**

-   The number of nodes in the tree is in the range `[1, 500]`.
-   `0 <= Node.val <= 500`
-   All the values `Node.val` are **unique**.
-   `target` is the value of one of the nodes in the tree.
-   `0 <= k <= 1000`

---
```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */
// Java HashMap + DFS 11ms(Beats 76.86%), Time O(N)
class Solution {
    List<Integer> ret = new ArrayList<>();
    HashMap<TreeNode, TreeNode> parents = new HashMap<>();
    Set<TreeNode> visited = new HashSet<>();
    public List<Integer> distanceK(TreeNode root, TreeNode target, int k) {
        findParents(root);
        dfs(target, k);
        return ret;
    }

    public void findParents(TreeNode node) {
        if (node == null) return;
        if (node.left != null) {
            parents.put(node.left, node);
            findParents(node.left);
        }
        if (node.right != null) {
            parents.put(node.right, node);
            findParents(node.right);
        }
    }

    public void dfs(TreeNode node, int k) {
        if (node == null || visited.contains(node)) return;
        visited.add(node);
        if (k == 0) {
            ret.add(node.val);
            return;
        }
        dfs(node.left, k - 1);
        dfs(node.right, k - 1);
        dfs(parents.get(node), k - 1);

    }
}
```


```java
// Java DFS 10ms(Beats 96.78%), Time O(N)
class Solution {
    List<Integer> ret = new ArrayList<>();
    public List<Integer> distanceK(TreeNode root, TreeNode target, int k) {
        dfs(root, target, k);
        return ret;
    }

    public int dfs(TreeNode root, TreeNode target, int k) {
        if (root == null) return -1;
        if (root == target) {
            depth(target, k);
            return 0;
        }
        int leftDis = dfs(root.left, target, k);
        if (leftDis != -1) {    // -1 = can't reach target
            if (leftDis + 1 == k) {
                ret.add(root.val);
            } else {
                depth(root.right, k - leftDis - 2);
            }
            return leftDis + 1;
        }

        int rightDis = dfs(root.right, target, k);
        if (rightDis != -1) {
            if (rightDis + 1 == k) {
                ret.add(root.val);
            } else {
                depth(root.left, k - rightDis - 2);
            }
            return rightDis + 1;
        }

        return -1;
    }

    // go downward from node, and distance is dis
    public void depth(TreeNode node, int dis) {
        if (dis < 0) return;
        if (node == null) return;
        if (dis == 0)
            ret.add(node.val);
        depth(node.left, dis - 1);
        depth(node.right, dis - 1);
    }
}
```


---

###### tags: `Leetcode` `Tree`
