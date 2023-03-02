# Leetcode - 815. Bus Routes (M+)

[Leetcode](https://leetcode.com/problems/bus-routes/description/)

You are given an array `routes` representing bus routes where `routes[i]` is a bus route that the `ith` bus repeats forever.

-   For example, if `routes[0] = [1, 5, 7]`, this means that the `0th` bus travels in the sequence `1 -> 5 -> 7 -> 1 -> 5 -> 7 -> 1 -> ...` forever.

You will start at the bus stop `source` (You are not on any bus initially), and you want to go to the bus stop `target`. You can travel between bus stops by buses only.

Return _the least number of buses you must take to travel from _`source`_ to _`target`. Return `-1` if it is not possible.

**Example 1:**
```
Input: routes = [[1,2,7],[3,6,7]], source = 1, target = 6
Output: 2
Explanation: The best strategy is take the first bus to the bus stop 7, then take the second bus to the bus stop 6.
```
**Example 2:**
```
Input: routes = [[7,12],[4,5,15],[6],[15,19],[9,12,13]], source = 15, target = 12
Output: -1
```
**Constraints:**

-   `1 <= routes.length <= 500`.
-   `1 <= routes[i].length <= 105`
-   All the values of `routes[i]` are **unique**.
-   `sum(routes[i].length) <= 105`
-   `0 <= routes[i][j] < 106`
-   `0 <= source, target < 106`

---
```java
// Java Hash 130ms
class Solution {
    public int numBusesToDestination(int[][] routes, int source, int target) {
        HashMap<Integer, HashSet<Integer>> stop2bus = new HashMap<>();
        HashSet<Integer> visitedBus = new HashSet<>();
        HashSet<Integer> visitedStop = new HashSet<>();
        int ret = -1;
        for (int i = 0; i < routes.length; i++) {
            for (int j : routes[i]) {
                stop2bus.computeIfAbsent(j, value -> new HashSet<Integer>()).add(i);
            }
        }

        Queue<Integer> queue = new LinkedList<>();
        queue.offer(source);
        while (!queue.isEmpty()) {
            int size = queue.size();
            ret++;
            for (int i = 0; i < size; i++) {
                int curStop = queue.poll();
                if (curStop == target)
                    return ret;
                visitedStop.add(curStop);
                
                for (int bus : stop2bus.getOrDefault(curStop, new HashSet<Integer>())) {
                    if (visitedBus.contains(bus)) continue;
                    visitedBus.add(bus);
                    for (int stop : routes[bus]) {
                        if (visitedStop.contains(stop)) continue;
                        queue.offer(stop);
                    }
                }
            }
        }
        return -1;

    }
}
```

```java
// Java Array 40ms
class Solution {
    public int numBusesToDestination(int[][] routes, int source, int target) {
        ArrayList<Integer>[] stop2bus = new ArrayList[1000000];
        int ret = -1;
        for (int i = 0; i < routes.length; i++) {
            for (int j : routes[i]) {
                if (stop2bus[j] == null)
                    stop2bus[j] = new ArrayList<Integer>();
                stop2bus[j].add(i);
            }
        }

        boolean[] visitedBus = new boolean[routes.length];
        boolean[] visitedStop = new boolean[1000000];
        Queue<Integer> queue = new LinkedList<>();
        queue.offer(source);
        while (!queue.isEmpty()) {
            int size = queue.size();
            ret++;
            for (int i = 0; i < size; i++) {
                int curStop = queue.poll();
                if (curStop == target)
                    return ret;
                visitedStop[curStop] = true;
                
                for (int bus : stop2bus[curStop]) {
                    if (visitedBus[bus]) continue;
                    visitedBus[bus] = true;
                    for (int stop : routes[bus]) {
                        if (visitedStop[stop]) continue;
                        queue.offer(stop);
                    }
                }
            }
        }
        return -1;

    }
}
```

---

[wisdompeak/YouTube](https://www.youtube.com/watch?v=gSr3ii4ipsk)

BFS的level階層代表為搭乘幾個bus
需要能從stop取得buses, 以及該bus能去的stops
處理去除重複visit

###### tags: `Leetcode` `BFS`
