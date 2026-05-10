<div align="center">

# 🎒 Pattern 15 — 0/1 Knapsack (Dynamic Programming)

[![Difficulty](https://img.shields.io/badge/Difficulty-Medium-yellow?style=flat-square)](#)
[![Problems](https://img.shields.io/badge/Problems-12+-blue?style=flat-square)](#practice-problems)

> **Core Idea:** Fill a DP table where dp[i][w] = max value using first i items with weight limit w. Decide: include item or skip.

</div>

---

## 🧠 What is 0/1 Knapsack?

Classic optimization problem: given items with **weight and value**, pack into knapsack of capacity W to **maximize value**. Each item taken 0 or 1 times.

---

## 🔑 When to Use It?

```
✅ Knapsack with weight constraints
✅ Partition equal subset sum
✅ Target sum (+ items = target)
✅ Coin change (min coins)
✅ House robber variants
✅ Resource allocation
```

---

## 📐 DP Table Visualization

```
Items: [(w=1, v=1), (w=2, v=3), (w=3, v=4)]
Capacity: 4

    0   1   2   3   4  
0   0   0   0   0   0
1   0   1   1   1   1     (item 0)
2   0   1   3   4   4     (items 0,1)
3   0   1   3   4   5     (items 0,1,2)

dp[i][j] = max value using first i items, capacity j
```

---

## 📋 Template Code

### 0/1 Knapsack (2D DP)
```java
public int knapsack(int capacity, int[] weights, int[] values) {
    int n = weights.length;
    int[][] dp = new int[n + 1][capacity + 1];
    
    for (int i = 1; i <= n; i++) {
        for (int w = 1; w <= capacity; w++) {
            if (weights[i - 1] <= w) {
                // Include or exclude this item
                dp[i][w] = Math.max(
                    values[i - 1] + dp[i - 1][w - weights[i - 1]],  // include
                    dp[i - 1][w]                                      // exclude
                );
            } else {
                // Can't include, so skip
                dp[i][w] = dp[i - 1][w];
            }
        }
    }
    
    return dp[n][capacity];
}
```

### 0/1 Knapsack (1D DP - Space Optimized)
```java
public int knapsack1D(int capacity, int[] weights, int[] values) {
    int[] dp = new int[capacity + 1];
    
    for (int i = 0; i < weights.length; i++) {
        // Iterate backwards to avoid using same item twice
        for (int w = capacity; w >= weights[i]; w--) {
            dp[w] = Math.max(
                dp[w],                                    // exclude
                values[i] + dp[w - weights[i]]          // include
            );
        }
    }
    
    return dp[capacity];
}
```

---

## 🔬 Detailed Example — Partition Equal Subset Sum

**Problem:** Can we partition array into two subsets with equal sum?

**Input:** `[1,5,11,5]` → **Output:** `true` (partition: [11] and [5,5,1])

```java
public boolean canPartition(int[] nums) {
    int totalSum = 0;
    for (int num : nums) {
        totalSum += num;
    }
    
    if (totalSum % 2 != 0) return false;
    
    int target = totalSum / 2;
    boolean[] dp = new boolean[target + 1];
    dp[0] = true;  // sum 0 always possible (empty subset)
    
    for (int num : nums) {
        for (int w = target; w >= num; w--) {
            dp[w] = dp[w] || dp[w - num];
        }
    }
    
    return dp[target];
}

// Trace for [1,5,11,5], target=11:
// Initial: dp[0]=true, rest=false
// Process 1: dp[1]=true
// Process 5: dp[5]=true, dp[6]=true
// Process 11: dp[11]=true
// Process 5: dp[11]=true (already)
// Result: dp[11]=true
```

---

## 💡 Key Insight

> **Backwards iteration in 1D.** Process capacity from high to low to ensure we don't use same item twice. dp[w] = max of (include current, exclude current).

---

## 🧩 Common Variations

| Variation | Modification |
|-----------|--------------|
| 0/1 Knapsack | Include/exclude each item once |
| Unbounded | Forward iteration to reuse items |
| Partition sum | Target = sum/2 |
| Coin change | Min coins to make amount |
| House robber | Max value, adjacent constraint |

---

## 🏋️ Practice Problems

### 🟡 Medium
| # | Problem | Platform | Key Trick |
|---|---------|----------|-----------|
| 1 | [0/1 Knapsack](https://www.educative.io/courses/grokking-dynamic-programming/RM53B1Z8j41) | Educative | Classic |
| 2 | [Partition Equal Subset](https://leetcode.com/problems/partition-equal-subset-sum/) | LeetCode #416 | Target = sum/2 |
| 3 | [Target Sum](https://leetcode.com/problems/target-sum/) | LeetCode #494 | (+/-) sum = target |
| 4 | [Coin Change 2](https://leetcode.com/problems/coin-change-2/) | LeetCode #518 | Ways to make amount |
| 5 | [House Robber](https://leetcode.com/problems/house-robber/) | LeetCode #198 | Can't rob adjacent |
| 6 | [Unbounded Knapsack](https://leetcode.com/problems/coin-change/) | LeetCode #322 | Reuse items |

---

## 📊 Complexity

| Operation | Time | Space |
|-----------|------|-------|
| Knapsack 2D | O(n·W) | O(n·W) |
| Knapsack 1D | O(n·W) | O(W) |
| Partition | O(n·sum/2) | O(sum/2) |

---

## 🔗 Navigation

[← K-Way Merge](./14-k-way-merge.md) | [Back to Main](../README.md) | [Next: Topological Sort →](./16-topological-sort.md)
