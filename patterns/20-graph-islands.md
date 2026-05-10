<div align="center">

# 🏝️ Pattern 20 — Graph Islands (DFS/BFS)

[![Difficulty](https://img.shields.io/badge/Difficulty-Medium-yellow?style=flat-square)](#)
[![Problems](https://img.shields.io/badge/Problems-12+-blue?style=flat-square)](#practice-problems)

> **Core Idea:** Traverse connected components using DFS/BFS. Mark visited to avoid revisits.

</div>

---

## 🧠 What is Graph Islands?

Find connected components in undirected graphs (grid or adjacency list). Use **DFS or BFS to explore each component**, marking visited cells/nodes.

---

## 🔑 When to Use It?

```
✅ Number of islands
✅ Largest island area
✅ Connected components
✅ Surrounded regions
✅ Number of distinct islands
✅ Robot return to origin
✅ Rotting oranges
```

---

## 📐 ASCII Visualization

```
Grid:
1 1 0 0 0
1 0 0 1 1
1 0 0 1 1

Islands: 3 (connected 1's)
Visited: mark as explored
```

---

## 📋 Template Code

### Number of Islands (DFS)
```java
public int numIslands(char[][] grid) {
    if (grid == null || grid.length == 0) return 0;
    
    int count = 0;
    int m = grid.length, n = grid[0].length;
    
    for (int i = 0; i < m; i++) {
        for (int j = 0; j < n; j++) {
            if (grid[i][j] == '1') {
                dfs(grid, i, j);
                count++;
            }
        }
    }
    
    return count;
}

private void dfs(char[][] grid, int i, int j) {
    if (i < 0 || i >= grid.length || j < 0 || j >= grid[0].length || 
        grid[i][j] != '1') {
        return;
    }
    
    grid[i][j] = '0';  // mark visited
    dfs(grid, i + 1, j);
    dfs(grid, i - 1, j);
    dfs(grid, i, j + 1);
    dfs(grid, i, j - 1);
}
```

### Max Island Area (BFS)
```java
public int maxAreaOfIsland(int[][] grid) {
    if (grid == null || grid.length == 0) return 0;
    
    int maxArea = 0;
    int m = grid.length, n = grid[0].length;
    
    for (int i = 0; i < m; i++) {
        for (int j = 0; j < n; j++) {
            if (grid[i][j] == 1) {
                maxArea = Math.max(maxArea, bfs(grid, i, j));
            }
        }
    }
    
    return maxArea;
}

private int bfs(int[][] grid, int si, int sj) {
    Queue<int[]> queue = new LinkedList<>();
    queue.offer(new int[] {si, sj});
    grid[si][sj] = 0;  // mark visited
    int area = 0;
    
    int[][] dirs = {{1, 0}, {-1, 0}, {0, 1}, {0, -1}};
    
    while (!queue.isEmpty()) {
        int[] cell = queue.poll();
        int i = cell[0], j = cell[1];
        area++;
        
        for (int[] dir : dirs) {
            int ni = i + dir[0], nj = j + dir[1];
            if (ni >= 0 && ni < grid.length && nj >= 0 && nj < grid[0].length && 
                grid[ni][nj] == 1) {
                grid[ni][nj] = 0;
                queue.offer(new int[] {ni, nj});
            }
        }
    }
    
    return area;
}
```

---

## 🔬 Detailed Example — Rotting Oranges

**Problem:** Oranges rot. Rotten spreads to adjacent fresh oranges in 1 minute. Return min minutes to rot all.

**Input:** `[[2,1,1],[1,1,0],[0,1,1]]` → **Output:** `4`

```java
public int orangesRotting(int[][] grid) {
    int m = grid.length, n = grid[0].length;
    Queue<int[]> queue = new LinkedList<>();
    int freshCount = 0;
    
    for (int i = 0; i < m; i++) {
        for (int j = 0; j < n; j++) {
            if (grid[i][j] == 2) {
                queue.offer(new int[] {i, j});
            } else if (grid[i][j] == 1) {
                freshCount++;
            }
        }
    }
    
    if (freshCount == 0) return 0;
    
    int minutes = 0;
    int[][] dirs = {{1, 0}, {-1, 0}, {0, 1}, {0, -1}};
    
    while (!queue.isEmpty()) {
        int size = queue.size();
        boolean rotted = false;
        
        for (int i = 0; i < size; i++) {
            int[] cell = queue.poll();
            int r = cell[0], c = cell[1];
            
            for (int[] dir : dirs) {
                int nr = r + dir[0], nc = c + dir[1];
                if (nr >= 0 && nr < m && nc >= 0 && nc < n && grid[nr][nc] == 1) {
                    grid[nr][nc] = 2;
                    freshCount--;
                    queue.offer(new int[] {nr, nc});
                    rotted = true;
                }
            }
        }
        
        if (rotted) minutes++;
    }
    
    return freshCount == 0 ? minutes : -1;
}

// Trace for [[2,1,1],[1,1,0],[0,1,1]]:
// Initial: queue=[2 at (0,0)], freshCount=5
// Minute 1: rot (0,1), (1,0)
// Minute 2: rot (0,2), (1,1)
// Minute 3: rot (1,2), (2,1)
// Minute 4: rot (2,2)
// freshCount=0 → return 4
```

---

## 💡 Key Insight

> **Mark as visited during traversal.** DFS recursive or BFS with queue. Both O(m·n). BFS helpful for level/time tracking.

---

## 🧩 Common Variations

| Problem | Approach |
|---------|----------|
| Count islands | DFS/BFS from unvisited |
| Max area | Track area during traversal |
| Surrounded regions | Reverse: mark safe regions |
| Rotting | BFS with levels |

---

## 🏋️ Practice Problems

### 🟢 Easy
| # | Problem | Platform | Key Trick |
|---|---------|----------|-----------|
| 1 | [Number of Islands](https://leetcode.com/problems/number-of-islands/) | LeetCode #200 | Basic DFS |

### 🟡 Medium
| # | Problem | Platform | Key Trick |
|---|---------|----------|-----------|
| 2 | [Max Island Area](https://leetcode.com/problems/max-area-of-island/) | LeetCode #695 | Track area |
| 3 | [Rotting Oranges](https://leetcode.com/problems/rotting-oranges/) | LeetCode #994 | BFS levels |
| 4 | [Surrounded Regions](https://leetcode.com/problems/surrounded-regions/) | LeetCode #130 | Reverse approach |
| 5 | [Number of Distinct Islands](https://leetcode.com/problems/number-of-distinct-islands/) | LeetCode #694 | Normalize paths |

---

## 📊 Complexity

| Operation | Time | Space |
|-----------|------|-------|
| Count islands | O(m·n) | O(m·n) visited |
| Max area | O(m·n) | O(m·n) |
| Rotting | O(m·n) | O(m·n) queue |

---

## 🔗 Navigation

[← Longest Common Substring](./19-longest-common-substring-dp.md) | [Back to Main](../README.md) | [Next: Monotonic Stack →](./21-monotonic-stack.md)
