<div align="center">

# 🔗 Pattern 16 — Topological Sort

[![Difficulty](https://img.shields.io/badge/Difficulty-Medium-yellow?style=flat-square)](#)
[![Problems](https://img.shields.io/badge/Problems-10+-blue?style=flat-square)](#practice-problems)

> **Core Idea:** Order nodes in a DAG (directed acyclic graph) such that for every edge u→v, u comes before v.

</div>

---

## 🧠 What is Topological Sort?

Order nodes such that **prerequisites are satisfied**. Two approaches: **Kahn's (BFS)** with in-degrees or **DFS** with backtracking.

---

## 🔑 When to Use It?

```
✅ Course schedule (prerequisites)
✅ Build order (dependencies)
✅ Alien dictionary (ordering chars)
✅ Task scheduling with dependencies
✅ Graph DAG validation
✅ Source code compilation
```

---

## 📐 ASCII Visualization

```
DAG:    1 → 2 → 3
        ↓   ↓
        4 → 5

Topological: [1, 2, 4, 3, 5] or [1, 4, 2, 5, 3]
(multiple valid orders)
```

---

## 📋 Template Code

### Kahn's Algorithm (BFS)
```java
public int[] topologicalSort(int n, int[][] edges) {
    int[] inDegree = new int[n];
    List<Integer>[] graph = new List[n];
    
    for (int i = 0; i < n; i++) {
        graph[i] = new ArrayList<>();
    }
    
    // Build graph
    for (int[] edge : edges) {
        int u = edge[0], v = edge[1];
        graph[u].add(v);
        inDegree[v]++;
    }
    
    Queue<Integer> queue = new LinkedList<>();
    for (int i = 0; i < n; i++) {
        if (inDegree[i] == 0) {
            queue.offer(i);
        }
    }
    
    int[] result = new int[n];
    int idx = 0;
    
    while (!queue.isEmpty()) {
        int u = queue.poll();
        result[idx++] = u;
        
        for (int v : graph[u]) {
            inDegree[v]--;
            if (inDegree[v] == 0) {
                queue.offer(v);
            }
        }
    }
    
    return idx == n ? result : new int[0];  // empty if cycle
}
```

### DFS Approach
```java
public int[] topologicalSortDFS(int n, int[][] edges) {
    List<Integer>[] graph = new List[n];
    int[] visited = new int[n];  // 0=unvisited, 1=visiting, 2=visited
    Stack<Integer> stack = new Stack<>();
    
    for (int i = 0; i < n; i++) {
        graph[i] = new ArrayList<>();
    }
    
    for (int[] edge : edges) {
        graph[edge[0]].add(edge[1]);
    }
    
    for (int i = 0; i < n; i++) {
        if (visited[i] == 0) {
            if (!dfs(i, graph, visited, stack)) {
                return new int[0];  // cycle detected
            }
        }
    }
    
    int[] result = new int[n];
    int idx = 0;
    while (!stack.isEmpty()) {
        result[idx++] = stack.pop();
    }
    return result;
}

private boolean dfs(int node, List<Integer>[] graph, int[] visited, Stack<Integer> stack) {
    visited[node] = 1;  // mark visiting
    
    for (int neighbor : graph[node]) {
        if (visited[neighbor] == 1) {
            return false;  // back edge → cycle
        }
        if (visited[neighbor] == 0) {
            if (!dfs(neighbor, graph, visited, stack)) {
                return false;
            }
        }
    }
    
    visited[node] = 2;  // mark visited
    stack.push(node);
    return true;
}
```

---

## 🔬 Detailed Example — Course Schedule

**Problem:** Can you finish all courses? courses[i] = [ai, bi] means take bi before ai.

**Input:** `2, [[1,0]]` → **Output:** `true`

```java
public boolean canFinish(int numCourses, int[][] prerequisites) {
    int[] inDegree = new int[numCourses];
    List<Integer>[] graph = new List[numCourses];
    
    for (int i = 0; i < numCourses; i++) {
        graph[i] = new ArrayList<>();
    }
    
    for (int[] prereq : prerequisites) {
        int course = prereq[0], prereq_course = prereq[1];
        graph[prereq_course].add(course);
        inDegree[course]++;
    }
    
    Queue<Integer> queue = new LinkedList<>();
    for (int i = 0; i < numCourses; i++) {
        if (inDegree[i] == 0) {
            queue.offer(i);
        }
    }
    
    int completed = 0;
    while (!queue.isEmpty()) {
        int course = queue.poll();
        completed++;
        
        for (int nextCourse : graph[course]) {
            inDegree[nextCourse]--;
            if (inDegree[nextCourse] == 0) {
                queue.offer(nextCourse);
            }
        }
    }
    
    return completed == numCourses;  // cycle if completed < numCourses
}

// Trace for 2, [[1,0]]:
// graph[0]=[1], inDegree=[0,1]
// Queue: [0]
// Poll 0: add 1 to queue, inDegree[1]=0
// Queue: [1]
// Poll 1: queue empty
// Completed = 2 = numCourses → true
```

---

## 💡 Key Insight

> **In-degree=0 nodes are sources.** Process all sources, decrease neighbors' in-degrees. If all nodes processed, DAG is valid. If not, cycle exists.

---

## 🧩 Common Variations

| Variation | Approach |
|-----------|----------|
| Detect cycle | In-degree or DFS colors |
| Topological order | Kahn's or DFS stack |
| Lexicographic | Min-heap instead of queue |
| Build path | Track parent |

---

## 🏋️ Practice Problems

### 🟢 Easy
| # | Problem | Platform | Key Trick |
|---|---------|----------|-----------|
| 1 | [Course Schedule](https://leetcode.com/problems/course-schedule/) | LeetCode #207 | Cycle detection |

### 🟡 Medium
| # | Problem | Platform | Key Trick |
|---|---------|----------|-----------|
| 2 | [Course Schedule II](https://leetcode.com/problems/course-schedule-ii/) | LeetCode #210 | Topological order |
| 3 | [Alien Dictionary](https://leetcode.com/problems/alien-dictionary/) | LeetCode #269 | Build graph from order |
| 4 | [Sequence Reconstruction](https://leetcode.com/problems/sequence-reconstruction/) | LeetCode #444 | Validate |

### 🔴 Hard
| # | Problem | Platform | Key Trick |
|---|---------|----------|-----------|
| 5 | [Alien Dictionary](https://leetcode.com/problems/alien-dictionary/) | LeetCode #269 | Extract + sort |

---

## 📊 Complexity

| Operation | Time | Space |
|-----------|------|-------|
| Kahn's | O(V + E) | O(V + E) |
| DFS | O(V + E) | O(V) depth |

---

## 🔗 Navigation

[← 0/1 Knapsack](./15-01-knapsack-dp.md) | [Back to Main](../README.md) | [Next: Fibonacci DP →](./17-fibonacci-dp.md)
