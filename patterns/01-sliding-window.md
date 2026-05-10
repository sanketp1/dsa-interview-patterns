<div align="center">

# 🪟 Pattern 01 — Sliding Window

[![Difficulty](https://img.shields.io/badge/Difficulty-Easy%20to%20Medium-green?style=flat-square)](#)
[![Problems](https://img.shields.io/badge/Problems-15+-blue?style=flat-square)](#practice-problems)

> **Core Idea:** Use a window (subarray/substring) that slides over data to avoid redundant computation.

</div>

---

## 🧠 What is the Sliding Window Pattern?

Instead of recomputing a result from scratch for every subarray, a **sliding window** maintains a running result by:
- **Expanding** the window from the right
- **Shrinking** it from the left when a constraint is violated

This turns O(n²) brute force into **O(n)** — a massive win.

---

## 🔑 When to Use It?

Look for these signals in the problem statement:

```
✅ "maximum/minimum subarray of size K"
✅ "longest substring with at most K distinct characters"
✅ "smallest subarray with sum ≥ X"
✅ contiguous subarray or substring
✅ "window", "range", or "consecutive elements"
```

---

## 🔀 Two Types of Windows

| Type | Window Size | Example |
|------|-------------|---------|
| **Fixed** | Always K elements | Max sum of subarray of size K |
| **Dynamic** | Grows/shrinks based on condition | Longest substring without repeating chars |

---

## 📐 ASCII Visualization

```
Array:  [2, 1, 5, 1, 3, 2]   K = 3

Step 1:  [2, 1, 5] 1  3  2    sum = 8
Step 2:   2 [1, 5, 1] 3  2    sum = 7
Step 3:   2  1 [5, 1, 3] 2    sum = 9  ← max
Step 4:   2  1  5 [1, 3, 2]   sum = 6

Answer: 9
```

---

## 📋 Template Code

### Fixed Window
```java
public static int fixedWindow(int[] arr, int k) {
    int windowSum = 0;
    for (int i = 0; i < k; i++) {
        windowSum += arr[i];  // initial window
    }
    int maxSum = windowSum;

    for (int i = k; i < arr.length; i++) {
        windowSum += arr[i];       // add new element
        windowSum -= arr[i - k];   // remove old element
        maxSum = Math.max(maxSum, windowSum);
    }

    return maxSum;
}
```

### Dynamic Window
```java
public static int dynamicWindow(int[] arr, int target) {
    int left = 0;
    int windowSum = 0;
    int minLen = Integer.MAX_VALUE;

    for (int right = 0; right < arr.length; right++) {
        windowSum += arr[right];  // expand

        while (windowSum >= target) {  // shrink condition
            minLen = Math.min(minLen, right - left + 1);
            windowSum -= arr[left];
            left++;
        }
    }

    return minLen == Integer.MAX_VALUE ? 0 : minLen;
}
```

---

## 🔬 Detailed Example — Longest Substring Without Repeating Characters

**Problem:** Given a string, find the length of the longest substring without repeating characters.

**Input:** `"abcabcbb"` → **Output:** `3` (`"abc"`)

```java
public static int lengthOfLongestSubstring(String s) {
    Map<Character, Integer> charIndex = new HashMap<>();  // stores last seen index of char
    int left = 0;
    int maxLen = 0;

    for (int right = 0; right < s.length(); right++) {
        char c = s.charAt(right);
        // If char seen and within current window → shrink
        if (charIndex.containsKey(c) && charIndex.get(c) >= left) {
            left = charIndex.get(c) + 1;
        }

        charIndex.put(c, right);
        maxLen = Math.max(maxLen, right - left + 1);
    }

    return maxLen;
}

// Trace:
// s = "abcabcbb"
// right=0 'a': window=[a]         len=1
// right=1 'b': window=[ab]        len=2
// right=2 'c': window=[abc]       len=3
// right=3 'a': shrink! left→1, window=[bca]  len=3
// right=4 'b': shrink! left→2, window=[cab]  len=3
// ...
// Answer: 3
```

---

## 💡 Key Insight

> Think of the window as a **conveyor belt**: items come in from the right and fall off from the left. You maintain a running state (sum, count, frequency map) rather than recomputing each time.

---

## 🧩 Common Variations

| Variation | Extra Data Structure |
|-----------|---------------------|
| Track characters | `HashMap / Counter` |
| Track indices | `HashMap` |
| Track min/max in window | `Deque (Monotonic)` |
| Count distinct elements | `Set` |

---

## 🏋️ Practice Problems

### 🟢 Easy
| # | Problem | Platform | Key Trick |
|---|---------|----------|-----------|
| 1 | [Maximum Average Subarray I](https://leetcode.com/problems/maximum-average-subarray-i/) | LeetCode #643 | Fixed window of size K |
| 2 | [Contains Duplicate II](https://leetcode.com/problems/contains-duplicate-ii/) | LeetCode #219 | Fixed window with set |
| 3 | [Find All Anagrams in a String](https://leetcode.com/problems/find-all-anagrams-in-a-string/) | LeetCode #438 | Fixed window + frequency |

### 🟡 Medium
| # | Problem | Platform | Key Trick |
|---|---------|----------|-----------|
| 4 | [Longest Substring Without Repeating Characters](https://leetcode.com/problems/longest-substring-without-repeating-characters/) | LeetCode #3 | Dynamic window + hashmap |
| 5 | [Minimum Size Subarray Sum](https://leetcode.com/problems/minimum-size-subarray-sum/) | LeetCode #209 | Dynamic window |
| 6 | [Longest Substring with At Most K Distinct Characters](https://leetcode.com/problems/longest-substring-with-at-most-two-distinct-characters/) | LeetCode #340 | Dynamic window + counter |
| 7 | [Max Consecutive Ones III](https://leetcode.com/problems/max-consecutive-ones-iii/) | LeetCode #1004 | Dynamic window, count zeros |
| 8 | [Fruit Into Baskets](https://leetcode.com/problems/fruit-into-baskets/) | LeetCode #904 | At most 2 distinct |
| 9 | [Permutation in String](https://leetcode.com/problems/permutation-in-string/) | LeetCode #567 | Fixed window + freq match |

### 🔴 Hard
| # | Problem | Platform | Key Trick |
|---|---------|----------|-----------|
| 10 | [Minimum Window Substring](https://leetcode.com/problems/minimum-window-substring/) | LeetCode #76 | Dynamic + two freq maps |
| 11 | [Sliding Window Maximum](https://leetcode.com/problems/sliding-window-maximum/) | LeetCode #239 | Fixed window + monotonic deque |
| 12 | [Substring with Concatenation of All Words](https://leetcode.com/problems/substring-with-concatenation-of-all-words/) | LeetCode #30 | Fixed window + word map |

---

## 📊 Complexity

| | Fixed Window | Dynamic Window |
|-|-------------|----------------|
| **Time** | O(n) | O(n) |
| **Space** | O(1) | O(k) — for the hash map |

---

## 🔗 Navigation

[← Back to Main](../README.md) | [Next: Two Pointers →](./02-two-pointers.md)