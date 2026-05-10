<div align="center">

# 🔄 Pattern 05 — Cyclic Sort

[![Difficulty](https://img.shields.io/badge/Difficulty-Medium-yellow?style=flat-square)](#)
[![Problems](https://img.shields.io/badge/Problems-8+-blue?style=flat-square)](#practice-problems)

> **Core Idea:** Place each element at its correct index position, since elements are in range [1,n] or [0,n-1].

</div>

---

## 🧠 What is the Cyclic Sort Pattern?

Cyclic Sort is used when you have an array of numbers in a specific range (typically 1 to n, or 0 to n-1). Instead of using extra space or sorting, place each number at its **correct index** by swapping.

**Key condition:** All numbers are in the range and at most one number is missing/duplicate.

---

## 🔑 When to Use It?

```
✅ Array contains numbers in range [1,n] where n = array length
✅ Array contains numbers in range [0,n-1]
✅ Find missing number
✅ Find all duplicates
✅ Find disappeared numbers
✅ Find the duplicate number (single pass, O(1) space)
```

---

## 📐 ASCII Visualization

```
Array: [3, 1, 2]

Goal: Element at index i should be i+1
      [1, 2, 3]

Step 1: 3 should be at index 2 → swap(0, 2)
        [2, 1, 3]

Step 2: 2 should be at index 1 → swap(0, 1)
        [1, 2, 3]

Step 3: 1 is at correct position ✓

Done!
```

---

## 📋 Template Code

### Basic Cyclic Sort
```java
public static void cyclicSort(int[] nums) {
    int i = 0;
    while (i < nums.length) {
        int correctIndex = nums[i] - 1;  // nums[i] should be at index (nums[i] - 1)
        
        if (nums[i] != nums[correctIndex]) {
            // Swap
            int temp = nums[i];
            nums[i] = nums[correctIndex];
            nums[correctIndex] = temp;
        } else {
            i++;
        }
    }
}
```

### Find Missing Number
```java
public static int findMissing(int[] nums) {
    int i = 0;
    while (i < nums.length) {
        int correctIndex = nums[i];  // nums[i] should be at index nums[i] (0-indexed, range [0,n])
        
        if (nums[i] < nums.length && nums[i] != nums[correctIndex]) {
            // Swap
            int temp = nums[i];
            nums[i] = nums[correctIndex];
            nums[correctIndex] = temp;
        } else {
            i++;
        }
    }
    
    for (int j = 0; j < nums.length; j++) {
        if (nums[j] != j) {
            return j;  // missing number
        }
    }
    return nums.length;  // if all positions filled, missing is n
}
```

---

## 🔬 Detailed Example — Find All Missing Numbers

**Problem:** Given array with range [1,n], find all missing numbers.

**Input:** `[1, 1, 2]` (n = 3) → **Output:** `[3]`

```java
public static List<Integer> findDisappeared(int[] nums) {
    List<Integer> result = new ArrayList<>();
    int i = 0;
    
    // Place each number at its correct position
    while (i < nums.length) {
        int correctIndex = nums[i] - 1;  // nums[i] should be at index (nums[i] - 1)
        
        if (nums[i] != nums[correctIndex]) {
            // Swap
            int temp = nums[i];
            nums[i] = nums[correctIndex];
            nums[correctIndex] = temp;
        } else {
            i++;
        }
    }
    
    // Find positions with wrong numbers
    for (int j = 0; j < nums.length; j++) {
        if (nums[j] != j + 1) {
            result.add(j + 1);  // missing number
        }
    }
    
    return result;
}

// Trace:
// nums = [1, 1, 2]
// After sorting: [1, 2, 1]
// Position 2 has 1 instead of 3 → missing is 3
```

---

## 💡 Key Insight

> **In-place without sorting.** Since numbers are bounded to a specific range, each position has a "home" index. Use this to place elements in O(n) time with O(1) space.

---

## 🧩 Common Variations

| Variation | Approach |
|-----------|----------|
| Find missing | Cycle sort + scan for gaps |
| Find duplicates | Cycle sort + mark |
| Find first missing | Early exit after sort |
| Find all missing | Scan after sort |

---

## 🏋️ Practice Problems

### 🟡 Medium
| # | Problem | Platform | Key Trick |
|---|---------|----------|-----------|
| 1 | [First Missing Positive](https://leetcode.com/problems/first-missing-positive/) | LeetCode #41 | Cyclic sort in [1,n] |
| 2 | [Find Missing Number](https://leetcode.com/problems/missing-number/) | LeetCode #268 | Cycle sort range [0,n] |
| 3 | [Find All Duplicates](https://leetcode.com/problems/find-all-duplicates-in-an-array/) | LeetCode #442 | Cycle sort + mark |
| 4 | [Find Disappeared Numbers](https://leetcode.com/problems/find-all-numbers-disappeared-in-an-array/) | LeetCode #448 | Cycle sort + gaps |
| 5 | [Set Mismatch](https://leetcode.com/problems/set-mismatch/) | LeetCode #645 | One duplicate, one missing |
| 6 | [Find Duplicate Number](https://leetcode.com/problems/find-the-duplicate-number/) | LeetCode #287 | In-place, O(1) space |

---

## 📊 Complexity

| Operation | Time | Space |
|-----------|------|-------|
| Cyclic Sort | O(n) | O(1) |
| Find Missing | O(n) | O(1) |
| Find All Missing | O(n) | O(k) — result list |

---

## 🔗 Navigation

[← Merge Intervals](./04-merge-intervals.md) | [Back to Main](../README.md) | [Next: In-Place Reversal →](./06-in-place-reversal-linkedlist.md)
