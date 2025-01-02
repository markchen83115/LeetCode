# Leetcode - 倒序DFS

```java
void dfs(int node, HashMap<Integer, Deque<Integer>> nextMap, List<Integer> list)
{
    Deque<Integer> nextQueue = nextMap.get(node);
    while (nextQueue != null && !nextQueue.isEmpty())
    {
        int nxt = nextQueue.pollFirst();
        dfs(nxt, nextMap, list);
    }
    list.add(node);
}
// [11,5],[5,4],[4,5],[5,1]
```

參考[2097. Valid Arrangement of Pairs (H)](https://hackmd.io/qqp4-L4ORjSHNP6BCnAu7A?view)  


###### tags: `Leetcode` `Templates`