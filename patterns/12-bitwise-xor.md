<div align="center">

# ⊕ Pattern 12 — Bitwise XOR

[![Difficulty](https://img.shields.io/badge/Difficulty-Easy%20to%20Medium-green?style=flat-square)](#)
[![Problems](https://img.shields.io/badge/Problems-10+-blue?style=flat-square)](#practice-problems)

> **Core Idea:** XOR properties: a ⊕ a = 0, a ⊕ 0 = a, a ⊕ b = b ⊕ a (commutative). Use for finding missing/unique numbers.

</div>

---

## 🧠 What is Bitwise XOR?

XOR (exclusive OR) returns 1 when bits differ. Key insight: **XOR-ing a number with itself gives 0**, and **XOR-ing with 0 gives the number itself**.

---

## 🔑 When to Use It?

```
✅ Find single number (all others appear twice)
✅ Find two unique numbers
✅ Find missing number
✅ Find duplicate in array
✅ Complement of base 10 number
✅ Flip bits for greycode
```

---

## 📐 Visual XOR Truth Table

```
0 ⊕ 0 = 0
0 ⊕ 1 = 1
1 ⊕ 0 = 1
1 ⊕ 1 = 0

Key: Different → 1, Same → 0
```

---

## 📋 Template Code

### Single Number (All Others Appear Twice)
```java
public int singleNumber(int[] nums) {
    int result = 0;
    for (int num : nums) {
        result ^= num;  // pairs cancel out to 0, single remains
    }
    return result;
}
```

### Find Two Unique Numbers
```java
public int[] findTwoUnique(int[] nums) {
    // XOR all → result of XOR of two unique numbers
    int xorAll = 0;
    for (int num : nums) {
        xorAll ^= num;
    }
    
    // Find rightmost set bit (differentiates the two numbers)
    int rightmost = xorAll & (-xorAll);
    
    // Partition and XOR
    int num1 = 0, num2 = 0;
    for (int num : nums) {
        if ((num & rightmost) != 0) {
            num1 ^= num;
        } else {
            num2 ^= num;
        }
    }
    
    return new int[] {num1, num2};
}
```

---

## 🔬 Detailed Example — Single Number III

**Problem:** All numbers appear twice except two. Find the two unique numbers.

**Input:** `[1,2,1,3,2,5]` → **Output:** `[3,5]` (or `[5,3]`)

```java
public int[] singleNumberIII(int[] nums) {
    // Step 1: XOR all → xor of two unique numbers
    int xorAll = 0;
    for (int num : nums) {
        xorAll ^= num;
    }
    
    // Step 2: Find rightmost bit set (differentiates the two)
    int mask = xorAll & (-xorAll);  // isolate rightmost 1 bit
    
    // Step 3: Partition by this bit and XOR separately
    int num1 = 0, num2 = 0;
    for (int num : nums) {
        if ((num & mask) == 0) {
            num1 ^= num;
        } else {
            num2 ^= num;
        }
    }
    
    return new int[] {num1, num2};
}

// Trace for [1,2,1,3,2,5]:
// XOR all: 1^2^1^3^2^5 = 3^5 = 6 (binary: 011^101=110)
// Rightmost bit of 6: 010 (decimal 2)
// Partition: bit 0→[1,1,3,5], bit 1→[2,2]
// XOR each: 1^1^3^5=3, 2^2=0... (recalculate)
// Result: [3, 5]
```

---

## 💡 Key Insight

> **XOR cancels pairs.** All pairs disappear (become 0), unique elements remain. For two uniques, partition by any differing bit.

---

## 🧩 Common Variations

| Variation | Approach |
|-----------|----------|
| Single number | XOR all (pairs→0, unique remains) |
| Two unique | XOR all + partition by bit |
| Missing number | XOR array + [1..n] |
| Bit flips | Gray code or pattern |

---

## 🏋️ Practice Problems

### 🟢 Easy
| # | Problem | Platform | Key Trick |
|---|---------|----------|-----------|
| 1 | [Single Number](https://leetcode.com/problems/single-number/) | LeetCode #136 | XOR all |
| 2 | [Missing Number](https://leetcode.com/problems/missing-number/) | LeetCode #268 | XOR with index |

### 🟡 Medium
| # | Problem | Platform | Key Trick |
|---|---------|----------|-----------|
| 3 | [Single Number II](https://leetcode.com/problems/single-number-ii/) | LeetCode #137 | Count bits mod 3 |
| 4 | [Single Number III](https://leetcode.com/problems/single-number-iii/) | LeetCode #260 | Partition by bit |
| 5 | [Complement Base 10](https://leetcode.com/problems/complement-of-base-10-integer/) | LeetCode #1009 | XOR with all 1s |
| 6 | [Gray Code](https://leetcode.com/problems/gray-code/) | LeetCode #89 | Pattern + XOR |

---

## 📊 Complexity

| Operation | Time | Space |
|-----------|------|-------|
| Single number | O(n) | O(1) |
| Two unique | O(n) | O(1) |
| Missing | O(n) | O(1) |

---

## 🔗 Navigation

[← Modified Binary Search](./11-modified-binary-search.md) | [Back to Main](../README.md) | [Next: Top K Elements →](./13-top-k-elements.md)
