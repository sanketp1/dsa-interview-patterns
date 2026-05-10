<div align="center">

# 🔍 Pattern 11 — Modified Binary Search

[![Difficulty](https://img.shields.io/badge/Difficulty-Medium-yellow?style=flat-square)](#)
[![Problems](https://img.shields.io/badge/Problems-12+-blue?style=flat-square)](#practice-problems)

> **Core Idea:** Adapt binary search for variations: rotated arrays, finding boundaries, searching near elements.

</div>

---

## 🧠 What is Modified Binary Search?

Standard binary search for **sorted arrays** works fine. But modify the comparison logic for **rotated arrays, finding first/last occurrence, or searching with conditions**.

---

## 🔑 When to Use It?

```
✅ Search in rotated sorted array
✅ Find first/last position of target
✅ Search in unknown-size array
✅ Peak finding in mountain array
✅ Find minimum in rotated array
✅ Bitonic array search
```

---

## 📐 ASCII Visualization

```
Rotated: [4,5,6,7,0,1,2]

Find 0:
Pivot at 3, array rotated.
Decide which half is sorted.
```

---

## 📋 Template Code

### Search in Rotated Sorted Array
```java
public int search(int[] nums, int target) {
    int left = 0, right = nums.length - 1;
    
    while (left <= right) {
        int mid = left + (right - left) / 2;
        
        if (nums[mid] == target) return mid;
        
        // Determine which half is sorted
        if (nums[left] <= nums[mid]) {  // left half sorted
            if (nums[left] <= target && target < nums[mid]) {
                right = mid - 1;  // target in left half
            } else {
                left = mid + 1;   // search right
            }
        } else {  // right half sorted
            if (nums[mid] < target && target <= nums[right]) {
                left = mid + 1;   // target in right half
            } else {
                right = mid - 1;  // search left
            }
        }
    }
    
    return -1;
}
```

### Find First and Last Position
```java
public int[] searchRange(int[] nums, int target) {
    if (nums == null || nums.length == 0) return new int[] {-1, -1};
    
    int first = findFirst(nums, target);
    if (first == -1) return new int[] {-1, -1};
    int last = findLast(nums, target);
    
    return new int[] {first, last};
}

private int findFirst(int[] nums, int target) {
    int left = 0, right = nums.length - 1;
    int result = -1;
    
    while (left <= right) {
        int mid = left + (right - left) / 2;
        
        if (nums[mid] == target) {
            result = mid;
            right = mid - 1;  // continue searching left
        } else if (nums[mid] < target) {
            left = mid + 1;
        } else {
            right = mid - 1;
        }
    }
    
    return result;
}

private int findLast(int[] nums, int target) {
    int left = 0, right = nums.length - 1;
    int result = -1;
    
    while (left <= right) {
        int mid = left + (right - left) / 2;
        
        if (nums[mid] == target) {
            result = mid;
            left = mid + 1;  // continue searching right
        } else if (nums[mid] < target) {
            left = mid + 1;
        } else {
            right = mid - 1;
        }
    }
    
    return result;
}
```

---

## 🔬 Detailed Example — Peak Element

**Problem:** Find peak in mountain array (increase then decrease).

**Input:** `[0,1,0]` → **Output:** `1` (index of peak)

```java
public int findPeak(int[] nums) {
    int left = 0, right = nums.length - 1;
    
    while (left < right) {
        int mid = left + (right - left) / 2;
        
        if (nums[mid] > nums[mid + 1]) {
            right = mid;  // peak in left or at mid
        } else {
            left = mid + 1;  // peak in right
        }
    }
    
    return left;
}

// Trace for [0,1,0]:
// left=0, right=2
// mid=1, nums[1]=1 > nums[2]=0 → right=1
// left=0, right=1
// mid=0, nums[0]=0 < nums[1]=1 → left=1
// left==right=1 → return 1 (peak index)
```

---

## 💡 Key Insight

> **Decide search direction smartly.** In rotated array: which half is sorted? Is target in sorted half? Always maintain **left <= right** invariant for correct boundary.

---

## 🧩 Common Variations

| Variation | Approach |
|-----------|----------|
| Rotated sorted | Determine sorted half |
| First/last position | Store result, continue search |
| Peak element | Compare with next, shrink half |
| Minimum rotated | Find where order breaks |

---

## 🏋️ Practice Problems

### 🟡 Medium
| # | Problem | Platform | Key Trick |
|---|---------|----------|-----------|
| 1 | [Search Rotated Sorted Array](https://leetcode.com/problems/search-in-rotated-sorted-array/) | LeetCode #33 | Determine sorted half |
| 2 | [Find First Last Position](https://leetcode.com/problems/find-first-and-last-position-of-element-in-sorted-array/) | LeetCode #34 | Store + continue |
| 3 | [Find Peak Element](https://leetcode.com/problems/find-peak-element/) | LeetCode #162 | Compare mid/mid+1 |
| 4 | [Find Minimum Rotated](https://leetcode.com/problems/find-minimum-in-rotated-sorted-array/) | LeetCode #153 | Find break point |
| 5 | [Search in Rotated II](https://leetcode.com/problems/search-in-rotated-sorted-array-ii/) | LeetCode #81 | Handle duplicates |
| 6 | [Bitonic Array Search](https://leetcode.com/problems/search-in-a-mountain-array/) | LeetCode #852 | Find peak + search |

---

## 📊 Complexity

| Operation | Time | Space |
|-----------|------|-------|
| Rotated search | O(log n) | O(1) |
| First/last | O(log n) | O(1) |
| Peak element | O(log n) | O(1) |

---

## 🔗 Navigation

[← Subsets](./10-subsets.md) | [Back to Main](../README.md) | [Next: Bitwise XOR →](./12-bitwise-xor.md)
