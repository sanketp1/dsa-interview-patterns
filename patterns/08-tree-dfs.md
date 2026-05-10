<div align="center">

# 🌲 Pattern 08 — Tree DFS (Depth-First Traversal)

[![Difficulty](https://img.shields.io/badge/Difficulty-Medium-yellow?style=flat-square)](#)
[![Problems](https://img.shields.io/badge/Problems-15+-blue?style=flat-square)](#practice-problems)

> **Core Idea:** Use recursion (or stack) to go deep into the tree, process nodes as you backtrack.

</div>

---

## 🧠 What is Tree DFS?

Depth-First Search explores as **far as possible** along each branch before backtracking. Naturally recursive. Three variants: **Preorder, Inorder, Postorder**.

---

## 🔑 When to Use It?

```
✅ Preorder / Inorder / Postorder traversal
✅ Path sum from root to leaf
✅ Validate BST
✅ Lowest Common Ancestor (LCA)
✅ Serialize/deserialize tree
✅ Construct tree from traversals
✅ Check if tree is balanced
✅ Count nodes / sum values
```

---

## 📐 ASCII Visualization

```
Tree:        1
            / \
           2   3

Preorder  (Root, Left, Right):   [1, 2, 3]
Inorder   (Left, Root, Right):   [2, 1, 3]
Postorder (Left, Right, Root):   [2, 3, 1]
```

---

## 📋 Template Code

### Preorder Traversal
```java
public List<Integer> preorder(TreeNode root) {
    List<Integer> result = new ArrayList<>();
    preorderHelper(root, result);
    return result;
}

private void preorderHelper(TreeNode node, List<Integer> result) {
    if (node == null) return;
    
    result.add(node.val);              // process root
    preorderHelper(node.left, result);  // traverse left
    preorderHelper(node.right, result); // traverse right
}
```

### Inorder Traversal
```java
public List<Integer> inorder(TreeNode root) {
    List<Integer> result = new ArrayList<>();
    inorderHelper(root, result);
    return result;
}

private void inorderHelper(TreeNode node, List<Integer> result) {
    if (node == null) return;
    
    inorderHelper(node.left, result);   // traverse left
    result.add(node.val);               // process root
    inorderHelper(node.right, result);  // traverse right
}
```

### Postorder Traversal
```java
public List<Integer> postorder(TreeNode root) {
    List<Integer> result = new ArrayList<>();
    postorderHelper(root, result);
    return result;
}

private void postorderHelper(TreeNode node, List<Integer> result) {
    if (node == null) return;
    
    postorderHelper(node.left, result);  // traverse left
    postorderHelper(node.right, result); // traverse right
    result.add(node.val);                // process root
}
```

---

## 🔬 Detailed Example — Path Sum

**Problem:** Find all root-to-leaf paths with given sum.

**Input:** Tree [5,4,8,11,null,13,4], targetSum=22 → **Output:** `[[5,4,11],[5,8,4,9]]`

```java
public List<List<Integer>> pathSum(TreeNode root, int targetSum) {
    List<List<Integer>> result = new ArrayList<>();
    pathSumHelper(root, targetSum, new ArrayList<>(), result);
    return result;
}

private void pathSumHelper(TreeNode node, int targetSum, List<Integer> path, List<List<Integer>> result) {
    if (node == null) return;
    
    path.add(node.val);
    targetSum -= node.val;
    
    // Leaf node and sum matches
    if (node.left == null && node.right == null && targetSum == 0) {
        result.add(new ArrayList<>(path));
    }
    
    // Continue searching
    pathSumHelper(node.left, targetSum, path, result);
    pathSumHelper(node.right, targetSum, path, result);
    
    path.remove(path.size() - 1);  // backtrack
}

// Trace for [5,4,8,11,null,13,4], targetSum=22:
// Path: 5 → 4 → 11 → sum=20, not leaf
// Path: 5 → 4 → 11 → 2 → sum=22, leaf ✓ → add [5,4,11,2]
// Path: 5 → 8 → 4 → 9 → sum=22, leaf ✓ → add [5,8,4,9]
```

---

## 💡 Key Insight

> **Recursion handles backtracking.** DFS + mutable state (path) = explore all possibilities. Remove from path before return (cleanup for next sibling).

---

## 🧩 Common Variations

| Variation | Approach |
|-----------|----------|
| Preorder | Root first |
| Inorder | Left-Root-Right (BST gives sorted) |
| Postorder | Children first (cleanup/aggregation) |
| Path queries | Pass state down, backtrack up |

---

## 🏋️ Practice Problems

### 🟢 Easy
| # | Problem | Platform | Key Trick |
|---|---------|----------|-----------|
| 1 | [Binary Tree Inorder](https://leetcode.com/problems/binary-tree-inorder-traversal/) | LeetCode #94 | Inorder traversal |
| 2 | [Path Sum](https://leetcode.com/problems/path-sum/) | LeetCode #112 | Check existence |
| 3 | [Maximum Depth](https://leetcode.com/problems/maximum-depth-of-binary-tree/) | LeetCode #104 | Return + recurse |

### 🟡 Medium
| # | Problem | Platform | Key Trick |
|---|---------|----------|-----------|
| 4 | [Path Sum II](https://leetcode.com/problems/path-sum-ii/) | LeetCode #113 | Collect all paths |
| 5 | [Validate BST](https://leetcode.com/problems/validate-binary-search-tree/) | LeetCode #98 | Min/max bounds |
| 6 | [Lowest Common Ancestor](https://leetcode.com/problems/lowest-common-ancestor-of-a-binary-tree/) | LeetCode #236 | Post-order logic |
| 7 | [Binary Tree Maximum Path Sum](https://leetcode.com/problems/binary-tree-maximum-path-sum/) | LeetCode #124 | Return max down path |

### 🔴 Hard
| # | Problem | Platform | Key Trick |
|---|---------|----------|-----------|
| 8 | [Serialize Deserialize](https://leetcode.com/problems/serialize-and-deserialize-binary-tree/) | LeetCode #297 | Preorder + null markers |

---

## 📊 Complexity

| Operation | Time | Space |
|-----------|------|-------|
| Traversal | O(n) | O(h) — recursion depth |
| Path sum | O(n) | O(h) + output |

---

## 🔗 Navigation

[← Tree BFS](./07-tree-bfs.md) | [Back to Main](../README.md) | [Next: Two Heaps →](./09-two-heaps.md)
