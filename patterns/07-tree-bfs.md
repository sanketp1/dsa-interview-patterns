<div align="center">

# 🌳 Pattern 07 — Tree BFS (Level-Order Traversal)

[![Difficulty](https://img.shields.io/badge/Difficulty-Medium-yellow?style=flat-square)](#)
[![Problems](https://img.shields.io/badge/Problems-12+-blue?style=flat-square)](#practice-problems)

> **Core Idea:** Use a queue to visit tree nodes level by level, left to right.

</div>

---

## 🧠 What is Tree BFS?

Breadth-First Search processes nodes **level by level** using a **queue**. Start from root, add children to queue, process next level.

---

## 🔑 When to Use It?

```
✅ Level-order traversal (print level by level)
✅ Find level with max/min sum
✅ Binary tree right view
✅ Average at each level
✅ Connect nodes at same level
✅ Zigzag traversal
✅ Count complete tree nodes
```

---

## 📐 ASCII Visualization

```
Tree:        1
            / \
           2   3
          / \   \
         4   5   6

BFS Order: [1, 2, 3, 4, 5, 6]
Level 0: [1]
Level 1: [2, 3]
Level 2: [4, 5, 6]
```

---

## 📋 Template Code

### Level-Order Traversal
```java
public List<List<Integer>> levelOrder(TreeNode root) {
    List<List<Integer>> result = new ArrayList<>();
    if (root == null) return result;
    
    Queue<TreeNode> queue = new LinkedList<>();
    queue.offer(root);
    
    while (!queue.isEmpty()) {
        int levelSize = queue.size();  // important: process level by level
        List<Integer> currentLevel = new ArrayList<>();
        
        for (int i = 0; i < levelSize; i++) {
            TreeNode node = queue.poll();
            currentLevel.add(node.val);
            
            if (node.left != null) queue.offer(node.left);
            if (node.right != null) queue.offer(node.right);
        }
        
        result.add(currentLevel);
    }
    
    return result;
}
```

### Binary Tree Right View
```java
public List<Integer> rightSideView(TreeNode root) {
    List<Integer> result = new ArrayList<>();
    if (root == null) return result;
    
    Queue<TreeNode> queue = new LinkedList<>();
    queue.offer(root);
    
    while (!queue.isEmpty()) {
        int levelSize = queue.size();
        
        for (int i = 0; i < levelSize; i++) {
            TreeNode node = queue.poll();
            
            if (i == levelSize - 1) {  // rightmost node at this level
                result.add(node.val);
            }
            
            if (node.left != null) queue.offer(node.left);
            if (node.right != null) queue.offer(node.right);
        }
    }
    
    return result;
}
```

---

## 🔬 Detailed Example — Average at Each Level

**Problem:** Find the average value of each level in a binary tree.

**Input:** Tree [3,9,20,null,null,15,7] → **Output:** `[3.0, 14.5, 6.0]`

```java
public List<Double> averageOfLevels(TreeNode root) {
    List<Double> result = new ArrayList<>();
    if (root == null) return result;
    
    Queue<TreeNode> queue = new LinkedList<>();
    queue.offer(root);
    
    while (!queue.isEmpty()) {
        int levelSize = queue.size();
        double sum = 0;
        
        for (int i = 0; i < levelSize; i++) {
            TreeNode node = queue.poll();
            sum += node.val;
            
            if (node.left != null) queue.offer(node.left);
            if (node.right != null) queue.offer(node.right);
        }
        
        result.add(sum / levelSize);
    }
    
    return result;
}

// Trace for [3,9,20,null,null,15,7]:
//        3        (level 0: sum=3, avg=3.0)
//       / \
//      9   20    (level 1: sum=29, avg=14.5)
//         / \
//        15  7   (level 2: sum=22, avg=11.0)
```

---

## 💡 Key Insight

> **Queue tracks positions, levelSize tracks boundaries.** Store levelSize before inner loop to process exact level then move to next.

---

## 🧩 Common Variations

| Variation | Approach |
|-----------|----------|
| Level-order | Standard BFS |
| Right view | Take rightmost at each level |
| Left view | Take leftmost at each level |
| Level sums | Sum all at level |
| Zigzag | Alternate direction per level |

---

## 🏋️ Practice Problems

### 🟢 Easy
| # | Problem | Platform | Key Trick |
|---|---------|----------|-----------|
| 1 | [Binary Tree Level Order](https://leetcode.com/problems/binary-tree-level-order-traversal/) | LeetCode #102 | Standard BFS |
| 2 | [Average of Levels](https://leetcode.com/problems/average-of-levels-in-binary-tree/) | LeetCode #637 | Sum + divide |

### 🟡 Medium
| # | Problem | Platform | Key Trick |
|---|---------|----------|-----------|
| 3 | [Binary Tree Right View](https://leetcode.com/problems/binary-tree-right-side-view/) | LeetCode #199 | Last at each level |
| 4 | [Level Order II](https://leetcode.com/problems/binary-tree-level-order-traversal-ii/) | LeetCode #107 | Reverse result |
| 5 | [Zigzag Level Order](https://leetcode.com/problems/binary-tree-zigzag-level-order-traversal/) | LeetCode #103 | Alternate direction |
| 6 | [Connect Next Pointers](https://leetcode.com/problems/populating-next-right-pointers-in-each-node/) | LeetCode #116 | Connect while BFS |

### 🔴 Hard
| # | Problem | Platform | Key Trick |
|---|---------|----------|-----------|
| 7 | [Count Complete Tree Nodes](https://leetcode.com/problems/count-complete-tree-nodes/) | LeetCode #222 | BFS + last level |

---

## 📊 Complexity

| Operation | Time | Space |
|-----------|------|-------|
| Level-order | O(n) | O(w) — w = max width |
| Right view | O(n) | O(w) |
| Average | O(n) | O(w) |

---

## 🔗 Navigation

[← In-Place Reversal](./06-in-place-reversal-linkedlist.md) | [Back to Main](../README.md) | [Next: Tree DFS →](./08-tree-dfs.md)
