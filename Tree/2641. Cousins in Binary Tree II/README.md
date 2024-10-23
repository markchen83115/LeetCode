# Leetcode - 2641. Cousins in Binary Tree II (M)

[Leetcode](https://leetcode.com/problems/cousins-in-binary-tree-ii/)

Given the `root` of a binary tree, replace the value of each node in the tree with the **sum of all its cousins' values**.

Two nodes of a binary tree are **cousins** if they have the same depth with different parents.

Return _the _`root`_ of the modified tree_.

**Note** that the depth of a node is the number of edges in the path from the root node to it.

**Example 1:**

![](https://assets.leetcode.com/uploads/2023/01/11/example11.png)
```
Input: root = [5,4,9,1,10,null,7]
Output: [0,0,0,7,7,null,11]
Explanation: The diagram above shows the initial binary tree and the binary tree after changing the value of each node.
- Node with value 5 does not have any cousins so its sum is 0.
- Node with value 4 does not have any cousins so its sum is 0.
- Node with value 9 does not have any cousins so its sum is 0.
- Node with value 1 has a cousin with value 7 so its sum is 7.
- Node with value 10 has a cousin with value 7 so its sum is 7.
- Node with value 7 has cousins with values 1 and 10 so its sum is 11.
```
**Example 2:**

![](https://assets.leetcode.com/uploads/2023/01/11/diagram33.png)
```
Input: root = [3,1,2]
Output: [0,0,0]
Explanation: The diagram above shows the initial binary tree and the binary tree after changing the value of each node.
- Node with value 3 does not have any cousins so its sum is 0.
- Node with value 1 does not have any cousins so its sum is 0.
- Node with value 2 does not have any cousins so its sum is 0.
```
**Constraints:**

-   The number of nodes in the tree is in the range `[1, 105]`.
-   `1 <= Node.val <= 104`

---
```java
// Java DFS 72ms(Beats 17.80%), Time O(N), Space O(N)
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
    int[] depthSum = new int[100005];
    HashMap<TreeNode, Integer> map = new HashMap<>();
    public TreeNode replaceValueInTree(TreeNode root) {
        map.put(root, root.val);
        dfsLevelSum(root, 0);
        dfsUpdateTree(root, 0);
        return root;
    }

    void dfsLevelSum(TreeNode root, int depth)
    {
        if (root == null) 
            return;
        depthSum[depth] += root.val;

        int leafSum = 0;
        if (root.left != null)
            leafSum += root.left.val;
        if (root.right != null)
            leafSum += root.right.val;

        if (root.left != null)
            map.put(root.left, leafSum);
        if (root.right != null)
            map.put(root.right, leafSum);

        dfsLevelSum(root.left, depth + 1);
        dfsLevelSum(root.right, depth + 1);
    }

    void dfsUpdateTree(TreeNode root, int depth)
    {
        if (root == null)
            return;
        root.val = depthSum[depth] - map.get(root);
        dfsUpdateTree(root.left, depth + 1);
        dfsUpdateTree(root.right, depth + 1);
    }
}
```
```java
// Java BFS 15ms(Beats 91.53%), Time O(N), Space O(N)
class Solution {
    public TreeNode replaceValueInTree(TreeNode root) {
        Queue<TreeNode> queue = new LinkedList<>();
        int currLevelSum = root.val;
        root.val *= -1;
        queue.offer(root);

        while (!queue.isEmpty())
        {
            int size = queue.size();
            int nextLevelSum = 0;
            while (size-- > 0)
            {
                TreeNode node = queue.poll();
                int childSum = 0;
                if (node.left != null) childSum += node.left.val;
                if (node.right != null) childSum += node.right.val;
                nextLevelSum += childSum;

                if (node.left != null) node.left.val = -childSum; 
                if (node.right != null) node.right.val = -childSum;
                node.val += currLevelSum;

                if (node.left != null) queue.offer(node.left);
                if (node.right != null) queue.offer(node.right);
            }

            currLevelSum = nextLevelSum;
        }

        return root;
    }
}
```
---

`DFS`:
求出各深度的總和, 並一併記錄每個`node`的`sibling node總和`
後續再一次`DFS`更新每個`node`

`BFS`:
求出當層深度的總和, 並作為下一層使用
並一併更新每個`node`, 將上一次層的加總扣掉`sibling node總和`


###### tags: `Leetcode` `Tree`