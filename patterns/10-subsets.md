<div align="center">

# 🎁 Pattern 10 — Subsets

[![Difficulty](https://img.shields.io/badge/Difficulty-Medium-yellow?style=flat-square)](#)
[![Problems](https://img.shields.io/badge/Problems-10+-blue?style=flat-square)](#practice-problems)

> **Core Idea:** Generate all combinations/permutations by recursively including/excluding elements or backtracking.

</div>

---

## 🧠 What is the Subsets Pattern?

Generate all possible subsets (power set), permutations, or combinations. Use **recursion + backtracking** to explore all choices.

---

## 🔑 When to Use It?

```
✅ Generate all subsets
✅ Generate all permutations
✅ Generate combinations (N choose K)
✅ Combination sum (with constraints)
✅ Permutation with duplicates
✅ Word search in grid
```

---

## 📐 ASCII Visualization

```
Subsets of [1, 2, 3]:
[]
[1]         [2]         [3]
[1,2]   [1,3]   [2,3]
[1,2,3]

Tree (recursive):
        []
       /  \
      /    \
    [1]    []
    / \    / \
 [1,2][1][2] []  ... (expand for 3)
```

---

## 📋 Template Code

### Generate All Subsets
```java
public List<List<Integer>> subsets(int[] nums) {
    List<List<Integer>> result = new ArrayList<>();
    backtrack(nums, 0, new ArrayList<>(), result);
    return result;
}

private void backtrack(int[] nums, int start, List<Integer> current, List<List<Integer>> result) {
    result.add(new ArrayList<>(current));  // add current subset
    
    for (int i = start; i < nums.length; i++) {
        current.add(nums[i]);               // include
        backtrack(nums, i + 1, current, result);  // recurse
        current.remove(current.size() - 1); // exclude (backtrack)
    }
}
```

### Generate All Permutations
```java
public List<List<Integer>> permute(int[] nums) {
    List<List<Integer>> result = new ArrayList<>();
    backtrackPermute(nums, new ArrayList<>(), new boolean[nums.length], result);
    return result;
}

private void backtrackPermute(int[] nums, List<Integer> current, boolean[] used, List<List<Integer>> result) {
    if (current.size() == nums.length) {
        result.add(new ArrayList<>(current));
        return;
    }
    
    for (int i = 0; i < nums.length; i++) {
        if (!used[i]) {
            current.add(nums[i]);
            used[i] = true;
            backtrackPermute(nums, current, used, result);
            current.remove(current.size() - 1);
            used[i] = false;
        }
    }
}
```

---

## 🔬 Detailed Example — Combination Sum

**Problem:** Find all unique combinations where numbers sum to target. Numbers can be reused.

**Input:** `nums=[2,3,6,7], target=7` → **Output:** `[[2,2,3],[7]]`

```java
public List<List<Integer>> combinationSum(int[] candidates, int target) {
    List<List<Integer>> result = new ArrayList<>();
    backtrackCombination(candidates, target, 0, new ArrayList<>(), result);
    return result;
}

private void backtrackCombination(int[] candidates, int target, int start, 
                                   List<Integer> current, List<List<Integer>> result) {
    if (target == 0) {
        result.add(new ArrayList<>(current));
        return;
    }
    
    if (target < 0) return;
    
    for (int i = start; i < candidates.length; i++) {
        current.add(candidates[i]);
        backtrackCombination(candidates, target - candidates[i], i, current, result);  // can reuse
        current.remove(current.size() - 1);
    }
}

// Trace for [2,3,6,7], target=7:
// Path: [2] → [2,2] → [2,2,3] ✓ sum=7
// Path: [2] → [2,3] → invalid (sum=5, try 6: too much)
// Path: [7] ✓ sum=7
```

---

## 💡 Key Insight

> **Backtrack = undo choice.** Include element, recurse with reduced target/updated state, then exclude (remove from list). Explore all branches.

---

## 🧩 Common Variations

| Variation | Key Difference |
|-----------|----------------|
| Subsets | Include/exclude each element |
| Permutations | Use all elements, track used |
| Combinations | No reuse, ordered |
| Combination Sum | Reuse allowed, target sum |

---

## 🏋️ Practice Problems

### 🟢 Easy
| # | Problem | Platform | Key Trick |
|---|---------|----------|-----------|
| 1 | [Subsets](https://leetcode.com/problems/subsets/) | LeetCode #78 | Basic backtrack |

### 🟡 Medium
| # | Problem | Platform | Key Trick |
|---|---------|----------|-----------|
| 2 | [Permutations](https://leetcode.com/problems/permutations/) | LeetCode #46 | Use all, track used |
| 3 | [Combination Sum](https://leetcode.com/problems/combination-sum/) | LeetCode #39 | Reuse, target |
| 4 | [Subsets II](https://leetcode.com/problems/subsets-ii/) | LeetCode #90 | Duplicates, skip |
| 5 | [Permutations II](https://leetcode.com/problems/permutations-ii/) | LeetCode #47 | Duplicates |
| 6 | [Combinations](https://leetcode.com/problems/combinations/) | LeetCode #77 | N choose K |

### 🔴 Hard
| # | Problem | Platform | Key Trick |
|---|---------|----------|-----------|
| 7 | [Word Search II](https://leetcode.com/problems/word-search-ii/) | LeetCode #212 | Backtrack grid |

---

## 📊 Complexity

| Operation | Time | Space |
|-----------|------|-------|
| Subsets | O(n · 2^n) | O(n) depth + output |
| Permutations | O(n! · n) | O(n) depth + output |
| Combinations | O(C(n,k) · k) | O(k) depth + output |

---

## 🔗 Navigation

[← Two Heaps](./09-two-heaps.md) | [Back to Main](../README.md) | [Next: Modified Binary Search →](./11-modified-binary-search.md)
