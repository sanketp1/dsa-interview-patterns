<div align="center">

# 🐇 Pattern 17 — Fibonacci Dynamic Programming

[![Difficulty](https://img.shields.io/badge/Difficulty-Easy-green?style=flat-square)](#)
[![Problems](https://img.shields.io/badge/Problems-8+-blue?style=flat-square)](#practice-problems)

> **Core Idea:** Optimal substructure + memoization. f(n) = f(n-1) + f(n-2). Build up from base cases.

</div>

---

## 🧠 What is Fibonacci DP?

Fibonacci-style problems have **linear recurrence** where each state depends on previous states. Memoize to avoid recomputation.

---

## 🔑 When to Use It?

```
✅ Climb stairs (1 or 2 steps)
✅ Decode ways
✅ House robber (can't rob adjacent)
✅ Min cost climbing
✅ Fibonacci number
✅ Regex matching
```

---

## 📐 Recurrence Visualization

```
f(5) = f(4) + f(3)
     = [f(3)+f(2)] + f(3)
     = f(3) + f(2) + f(3)

With memo: compute each once
f(0)=1, f(1)=1, f(2)=2, f(3)=3, f(4)=5, f(5)=8
```

---

## 📋 Template Code

### Climb Stairs (Top-Down Memoization)
```java
public int climbStairs(int n) {
    int[] memo = new int[n + 1];
    return dp(n, memo);
}

private int dp(int n, int[] memo) {
    if (n <= 1) return 1;
    if (memo[n] != 0) return memo[n];
    
    memo[n] = dp(n - 1, memo) + dp(n - 2, memo);
    return memo[n];
}
```

### Climb Stairs (Bottom-Up DP)
```java
public int climbStairsBottomUp(int n) {
    if (n <= 1) return 1;
    
    int[] dp = new int[n + 1];
    dp[1] = 1;
    dp[2] = 2;
    
    for (int i = 3; i <= n; i++) {
        dp[i] = dp[i - 1] + dp[i - 2];
    }
    
    return dp[n];
}
```

### Space-Optimized (2 Variables)
```java
public int climbStairsOptimized(int n) {
    if (n <= 1) return 1;
    
    int prev2 = 1, prev1 = 1;
    for (int i = 2; i <= n; i++) {
        int current = prev1 + prev2;
        prev2 = prev1;
        prev1 = current;
    }
    
    return prev1;
}
```

---

## 🔬 Detailed Example — House Robber

**Problem:** Max money robbing houses; can't rob adjacent houses.

**Input:** `[1,2,3,1]` → **Output:** `4` (rob house 1 and 3: 1+3)

```java
public int rob(int[] nums) {
    if (nums == null || nums.length == 0) return 0;
    if (nums.length == 1) return nums[0];
    
    int[] dp = new int[nums.length];
    dp[0] = nums[0];
    dp[1] = Math.max(nums[0], nums[1]);
    
    for (int i = 2; i < nums.length; i++) {
        // Rob current or skip current
        dp[i] = Math.max(
            nums[i] + dp[i - 2],  // rob current + max from i-2
            dp[i - 1]              // skip current, max from i-1
        );
    }
    
    return dp[nums.length - 1];
}

// Trace for [1,2,3,1]:
// dp[0] = 1
// dp[1] = max(1, 2) = 2
// dp[2] = max(3+1, 2) = 4
// dp[3] = max(1+4, 4) = 5 (wait, should be 4)
// Recalc: dp[3] = max(1+2, 4) = 4 ✓
```

---

## 💡 Key Insight

> **Memoization or Bottom-Up.** Avoid recomputation by storing results. Space-optimize later by keeping only last K states.

---

## 🧩 Common Variations

| Problem | Recurrence |
|---------|-----------|
| Climb stairs | dp[i] = dp[i-1] + dp[i-2] |
| House robber | dp[i] = max(nums[i] + dp[i-2], dp[i-1]) |
| Decode ways | dp[i] = dp[i-1] + dp[i-2] (if valid) |
| Min cost stairs | dp[i] = cost[i] + min(dp[i-1], dp[i-2]) |

---

## 🏋️ Practice Problems

### 🟢 Easy
| # | Problem | Platform | Key Trick |
|---|---------|----------|-----------|
| 1 | [Climb Stairs](https://leetcode.com/problems/climbing-stairs/) | LeetCode #70 | Basic fib |
| 2 | [Min Cost Climbing](https://leetcode.com/problems/min-cost-climbing-stairs/) | LeetCode #746 | Fib + cost |
| 3 | [House Robber](https://leetcode.com/problems/house-robber/) | LeetCode #198 | Max non-adjacent |

### 🟡 Medium
| # | Problem | Platform | Key Trick |
|---|---------|----------|-----------|
| 4 | [Decode Ways](https://leetcode.com/problems/decode-ways/) | LeetCode #91 | Conditional recurrence |
| 5 | [House Robber II](https://leetcode.com/problems/house-robber-ii/) | LeetCode #213 | Circular constraint |
| 6 | [Paint House](https://leetcode.com/problems/paint-house/) | LeetCode #256 | State-based |

---

## 📊 Complexity

| Approach | Time | Space |
|----------|------|-------|
| Top-down memo | O(n) | O(n) |
| Bottom-up | O(n) | O(n) |
| Optimized | O(n) | O(1) |

---

## 🔗 Navigation

[← Topological Sort](./16-topological-sort.md) | [Back to Main](../README.md) | [Next: Palindromic Subsequence →](./18-palindromic-subsequence-dp.md)
