# Leetcode - 3408. Design Task Manager (M+)

[Leetcode](https://leetcode.com/problems/design-task-manager/)

There is a task management system that allows users to manage their tasks, each associated with a priority. The system should efficiently handle adding, modifying, executing, and removing tasks.

Implement the `TaskManager` class:

-   `TaskManager(vector<vector<int>>& tasks)` initializes the task manager with a list of user-task-priority triples. Each element in the input list is of the form `[userId, taskId, priority]`, which adds a task to the specified user with the given priority.
    
-   `void add(int userId, int taskId, int priority)` adds a task with the specified `taskId` and `priority` to the user with `userId`. It is **guaranteed** that `taskId` does not _exist_ in the system.
    
-   `void edit(int taskId, int newPriority)` updates the priority of the existing `taskId` to `newPriority`. It is **guaranteed** that `taskId` _exists_ in the system.
    
-   `void rmv(int taskId)` removes the task identified by `taskId` from the system. It is **guaranteed** that `taskId` _exists_ in the system.
    
-   `int execTop()` executes the task with the **highest** priority across all users. If there are multiple tasks with the same **highest** priority, execute the one with the highest `taskId`. After executing, the `taskId` is **removed** from the system. Return the `userId` associated with the executed task. If no tasks are available, return -1.
    

**Note** that a user may be assigned multiple tasks.

**Example 1:**

> **Input:**  
> ["TaskManager", "add", "edit", "execTop", "rmv", "add", "execTop"]  
> [[[[1, 101, 10], [2, 102, 20], [3, 103, 15]]], [4, 104, 5], [102, 8], [], [101], [5, 105, 15], []]
> 
> **Output:**  
> [null, null, null, 3, null, null, 5]
> 
> **Explanation**
> 
> TaskManager taskManager = new TaskManager([[1, 101, 10], [2, 102, 20], [3, 103, 15]]); // Initializes with three tasks for Users 1, 2, and 3.  
> taskManager.add(4, 104, 5); // Adds task 104 with priority 5 for User 4.  
> taskManager.edit(102, 8); // Updates priority of task 102 to 8.  
> taskManager.execTop(); // return 3. Executes task 103 for User 3.  
> taskManager.rmv(101); // Removes task 101 from the system.  
> taskManager.add(5, 105, 15); // Adds task 105 with priority 15 for User 5.  
> taskManager.execTop(); // return 5. Executes task 105 for User 5.

**Constraints:**

-   `1 <= tasks.length <= 105`
-   `0 <= userId <= 105`
-   `0 <= taskId <= 105`
-   `0 <= priority <= 109`
-   `0 <= newPriority <= 109`
-   At most `2 * 105` calls will be made in **total** to `add`, `edit`, `rmv`, and `execTop` methods.
-   The input is generated such that `taskId` will be valid.

---
```java
// Java PriorityQueue 312ms(Beats 75.43%), Time O(logN), Space O(N)
class TaskManager {
    PriorityQueue<int[]> pq; // priority, taskId
    int[] t2p;
    int[] t2u;
    public TaskManager(List<List<Integer>> tasks) {
        pq = new PriorityQueue<>((a, b) -> b[0] == a[0] ? b[1] - a[1] : b[0] - a[0]);
        t2p = new int[100005];
        t2u = new int[100005];
        Arrays.fill(t2p, -1);
        Arrays.fill(t2u, -1);

        for (List<Integer> t : tasks) {
            int user = t.get(0), task = t.get(1), priority = t.get(2);
            t2p[task] = priority;
            t2u[task] = user;
            pq.offer(new int[] {priority, task});
        }
    }
    
    public void add(int userId, int taskId, int priority) {
        t2p[taskId] = priority;
        t2u[taskId] = userId;
        pq.offer(new int[] {priority, taskId});
    }
    
    public void edit(int taskId, int newPriority) {
        t2p[taskId] = newPriority;
        pq.offer(new int[] {newPriority, taskId});
    }
    
    public void rmv(int taskId) {
        t2p[taskId] = -1;
        t2u[taskId] = -1;
    }
    
    public int execTop() {
        while (!pq.isEmpty())
        {
            int[] curr = pq.poll();
            int priority = curr[0], task = curr[1];
            if (t2p[task] != -1 && t2p[task] == priority)
            {
                int user = t2u[task];
                rmv(task);
                return user;
            }
        }

        return -1; 
    }
}
```
```java
// Java TreeMap+Object 330ms(Beats 64.65%), Time O(logN), Space O(N)
class TaskManager {
    class Task {
        int user;
        int task;
        int priority;

        Task (int user, int task, int priority)
        {
            this.user = user;
            this.task = task;
            this.priority = priority;
        }
    }

    TreeMap<Task, Integer> map; // Task, userId
    Task[] tasks;
    public TaskManager(List<List<Integer>> tasks) {
        map = new TreeMap<>((a, b) -> b.priority == a.priority ? b.task - a.task : b.priority - a.priority);
        this.tasks = new Task[100005];

        for (List<Integer> t : tasks) {
            int userId = t.get(0), taskId = t.get(1), priority = t.get(2);
            add(userId, taskId, priority);
        }
    }
    
    public void add(int userId, int taskId, int priority) {
        Task task = new Task(userId, taskId, priority);
        map.put(task, userId);
        tasks[taskId] = task;
    }
    
    public void edit(int taskId, int newPriority) {
        Task t = tasks[taskId];
        if (t != null)
        {
            map.remove(t);
            t.priority = newPriority;
            map.put(t, t.user);
        }
    }
    
    public void rmv(int taskId) {
        Task t = tasks[taskId];
        map.remove(t);
        tasks[t.task] = null;
    }
    
    public int execTop() {
        if (map.isEmpty())
            return -1;

        Task t = map.firstKey();
        rmv(t.task);
        return t.user;
    }
}
```
---

透過PQ來實作比較priority
利用懶刪除, 檢查pop出來的task內容是否與該taskId所記錄的priority相同


###### tags: `Leetcode` `Priority Queue` `Design`