<div align="center">

# 👉 Pattern 02 — Two Pointers

[![Difficulty](https://img.shields.io/badge/Difficulty-Easy%20to%20Medium-green?style=flat-square)](#)
[![Problems](https://img.shields.io/badge/Problems-15+-blue?style=flat-square)](#practice-problems)

> **Core Idea:** Use two indices moving toward (or away from) each other to eliminate nested loops.

</div>

---

## 🧠 What is the Two Pointers Pattern?

Two Pointers is a technique where you place pointers at different positions in an array/string and **move them strategically** based on a condition. It collapses O(n²) brute force to **O(n)**.

---

## 🔑 When to Use It?

```
✅ Sorted array + target sum/pair
✅ Find triplets/quadruplets summing to target
✅ Remove duplicates in-place
✅ Palindrome check
✅ Container with most water
✅ Two arrays to merge or compare
```

---

## 🔀 Three Flavors

| Flavor | Movement | Use Case |
|--------|----------|----------|
| **Converging** | Start→End & End→Start | Pair sum, palindrome |
| **Same Direction** | Both move left→right | Remove duplicates, partition |
| **Two Arrays** | One pointer per array | Merge sorted arrays |

---

## 📐 ASCII Visualization

```
Pair Sum = 9
Array:  [1, 2, 4, 6, 8, 9]
         L              R

Step 1: L=1, R=9  → sum=10 > 9 → R--
Step 2: L=1, R=8  → sum=9 ✅  → Found!
```

---

## 📋 Template Code

### Converging Pointers (Pair Sum)
```java
public static int[] pairWithTargetSum(int[] arr, int target) {
    int left = 0, right = arr.length - 1;

    while (left < right) {
        int currentSum = arr[left] + arr[right];

        if (currentSum == target) {
            return new int[] {left, right};
        } else if (currentSum < target) {
            left++;       // need bigger sum
        } else {
            right--;      // need smaller sum
        }
    }

    return new int[] {};
}
```

### Same Direction (Remove Duplicates)
```java
public static int removeDuplicates(int[] arr) {
    if (arr == null || arr.length == 0) {
        return 0;
    }

    int slow = 0;   // next position for unique element

    for (int fast = 1; fast < arr.length; fast++) {
        if (arr[fast] != arr[slow]) {   // found new unique
            slow++;
            arr[slow] = arr[fast];
        }
    }

    return slow + 1;
}
```

---

## 🔬 Detailed Example — 3Sum

**Problem:** Find all unique triplets that sum to zero.

**Input:** `[-1, 0, 1, 2, -1, -4]` → **Output:** `[[-1,-1,2], [-1,0,1]]`

```java
public static List<List<Integer>> threeSum(int[] nums) {
    Arrays.sort(nums);
    List<List<Integer>> result = new ArrayList<>();

    for (int i = 0; i < nums.length - 2; i++) {
        if (i > 0 && nums[i] == nums[i - 1]) {  // skip duplicates
            continue;
        }

        int left = i + 1, right = nums.length - 1;

        while (left < right) {
            int total = nums[i] + nums[left] + nums[right];

            if (total == 0) {
                result.add(Arrays.asList(nums[i], nums[left], nums[right]));
                // skip duplicates
                while (left < right && nums[left] == nums[left + 1]) {
                    left++;
                }
                while (left < right && nums[right] == nums[right - 1]) {
                    right--;
                }
                left++;
                right--;
            } else if (total < 0) {
                left++;
            } else {
                right--;
            }
        }
    }

    return result;
}

// Time: O(n²)  Space: O(1) ignoring output
```

---

## 💡 Key Insight

> Sort first — then two pointers. Sorting gives you a **decision rule**: if sum is too small, move left pointer right. If too big, move right pointer left. That's it.

---

## 🧩 Common Variations

| Variation | Strategy |
|-----------|----------|
| Pair with target | Sort + converge |
| Triplets / Quadruplets | Fix outer + converge inner |
| Remove element in-place | Slow/fast same direction |
| Reverse array/string | Converge and swap |
| Container with most water | Converge, move smaller height |

---

## 🏋️ Practice Problems

### 🟢 Easy
| # | Problem | Platform | Key Trick |
|---|---------|----------|-----------|
| 1 | [Two Sum II - Sorted](https://leetcode.com/problems/two-sum-ii-input-array-is-sorted/) | LeetCode #167 | Classic converge |
| 2 | [Valid Palindrome](https://leetcode.com/problems/valid-palindrome/) | LeetCode #125 | Converge + skip non-alnum |
| 3 | [Merge Sorted Array](https://leetcode.com/problems/merge-sorted-array/) | LeetCode #88 | Two pointers from end |
| 4 | [Remove Duplicates from Sorted Array](https://leetcode.com/problems/remove-duplicates-from-sorted-array/) | LeetCode #26 | Slow/fast |
| 5 | [Squares of Sorted Array](https://leetcode.com/problems/squares-of-a-sorted-array/) | LeetCode #977 | Converge, fill from end |

### 🟡 Medium
| # | Problem | Platform | Key Trick |
|---|---------|----------|-----------|
| 6 | [3Sum](https://leetcode.com/problems/3sum/) | LeetCode #15 | Sort + fix + converge |
| 7 | [Container With Most Water](https://leetcode.com/problems/container-with-most-water/) | LeetCode #11 | Move smaller height |
| 8 | [Sort Colors (Dutch Flag)](https://leetcode.com/problems/sort-colors/) | LeetCode #75 | 3 pointers |
| 9 | [4Sum](https://leetcode.com/problems/4sum/) | LeetCode #18 | Fix two + converge |
| 10 | [Remove Nth Node From End](https://leetcode.com/problems/remove-nth-node-from-end-of-list/) | LeetCode #19 | Gap of N between pointers |
| 11 | [Backspace String Compare](https://leetcode.com/problems/backspace-string-compare/) | LeetCode #844 | Two pointers from end |

### 🔴 Hard
| # | Problem | Platform | Key Trick |
|---|---------|----------|-----------|
| 12 | [Trapping Rain Water](https://leetcode.com/problems/trapping-rain-water/) | LeetCode #42 | Converge + track max |
| 13 | [Minimum Window Substring](https://leetcode.com/problems/minimum-window-substring/) | LeetCode #76 | Sliding window variant |

---

## 📊 Complexity

| | Time | Space |
|-|------|-------|
| **Converging** | O(n) after O(n log n) sort | O(1) |
| **Same direction** | O(n) | O(1) |
| **3Sum** | O(n²) | O(1) |

---

## 🔗 Navigation

[← Sliding Window](./01-sliding-window.md) | [Back to Main](../README.md) | [Next: Fast & Slow Pointers →](./03-fast-slow-pointers.md)