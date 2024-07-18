# Leetcode - 1530. Number of Good Leaf Nodes Pairs (M)

[Leetcode](https://leetcode.com/problems/number-of-good-leaf-nodes-pairs/)

You are given the `root` of a binary tree and an integer `distance`. A pair of two different **leaf** nodes of a binary tree is said to be good if the length of **the shortest path** between them is less than or equal to `distance`.

Return _the number of good leaf node pairs_ in the tree.

**Example 1:**

![](https://assets.leetcode.com/uploads/2020/07/09/e1.jpg)
```
Input: root = [1,2,3,null,4], distance = 3
Output: 1
Explanation: The leaf nodes of the tree are 3 and 4 and the length of the shortest path between them is 3. This is the only good pair.
```
**Example 2:**

![](https://assets.leetcode.com/uploads/2020/07/09/e2.jpg)
```
Input: root = [1,2,3,4,5,6,7], distance = 3
Output: 2
Explanation: The good pairs are [4,5] and [6,7] with shortest path = 2. The pair [4,6] is not good because the length of ther shortest path between them is 4.
```
**Example 3:**
```
Input: root = [7,1,4,6,null,5,3,null,null,null,null,null,2], distance = 3
Output: 1
Explanation: The only good pair is [2,5].
```
**Constraints:**

-   The number of nodes in the `tree` is in the range `[1, 210].`
-   `1 <= Node.val <= 100`
-   `1 <= distance <= 10`

---
```java
// Java BFS 83ms(Beats 7.63%), Time O(N^2), Space O(N)
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
    Map<TreeNode, List<TreeNode>> graph = new HashMap<>();
    Set<TreeNode> leaves = new HashSet<>();
    public int countPairs(TreeNode root, int distance) {
        int ret = 0;
        toGraph(root, null);    //建立Graph

        for (TreeNode leaf : leaves)    //每一個leaf跑一次
        {   
            ArrayDeque<TreeNode> queue = new ArrayDeque<>();
            Set<TreeNode> visited = new HashSet<>();
            queue.offer(leaf);
            visited.add(leaf);
            for (int i = 0; i <= distance; i++)  //每一圈跑完 distance + 1
            {
                int size = queue.size();
                for (int j = 0; j < size; j++)
                {
                    TreeNode node = queue.poll();
                    if (leaves.contains(node) && node != leaf)
                        ret++;
                    if (graph.containsKey(node))
                    {
                        for (TreeNode nxt : graph.get(node))
                        {
                            if (!visited.contains(nxt))
                            {
                                queue.offer(nxt);
                                visited.add(nxt);
                            }
                        }
                    }
                }
            }
        }

        return ret / 2;
    }

    void toGraph(TreeNode root, TreeNode parent)
    {
        if (root == null)
            return;
        if (root.left == null && root.right == null)
            leaves.add(root);
        if (parent != null)
        {
            graph.computeIfAbsent(root, k -> new ArrayList<TreeNode>());
            graph.get(root).add(parent);
            graph.computeIfAbsent(parent, k -> new ArrayList<TreeNode>());
            graph.get(parent).add(root);
        }
        toGraph(root.left, root);
        toGraph(root.right, root);
    }   
}
```
```java
// Java DFS 39ms(Beats 12.03%), Time O(N), Space O(N)
class Solution {
    int count = 0;
    public int countPairs(TreeNode root, int distance) {
        dfs(root, distance);
        return count;
    }

    List<Integer> dfs(TreeNode root, int distance)
    {
        if (root == null)
            return new LinkedList<>();
        if (root.left == null && root.right == null)
        {
            List<Integer> ret = new ArrayList<>();
            ret.add(1);     // 1 leaf node which distance = 1
            return ret;
        }
        List<Integer> left = dfs(root.left, distance);
        List<Integer> right = dfs(root.right, distance);
        if (left.size() > 0 && right.size() > 0)
            for (int l : left)
                for (int r : right)
                    if (l + r <= distance)
                        count++;
        
        List<Integer> ret = new LinkedList<>();
        for (int l : left)
            ret.add(l + 1);
        for (int r : right)
            ret.add(r + 1);
        return ret;    
    }
}
```
---

#### BFS:
透過建立Graph(每個點的相鄰點有哪些), 並以每個leaf node為出發點,
算出在distance內的點有多少個, 因為會重複計算, 因此答案/2

#### DFS:
透過Post order Traversal(LRV), 
使左右node點回傳"每個leaf node與當前node的距離",
ex: `{1, 1, 2}`代表當前node底下有3個leaf node且距離分別為`1, 1, 2`
計算左邊leaf node + 右邊leaf node <= distance的數量

回傳時, 將所有左+右leaf node距離都+1,
因為有一個leaf node距離為i, 則lead node與parent node的距離為i+1


###### tags: `Leetcode` `Tree`