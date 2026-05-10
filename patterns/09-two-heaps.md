<div align="center">

# 🏔️ Pattern 09 — Two Heaps

[![Difficulty](https://img.shields.io/badge/Difficulty-Medium-yellow?style=flat-square)](#)
[![Problems](https://img.shields.io/badge/Problems-8+-blue?style=flat-square)](#practice-problems)

> **Core Idea:** Maintain two heaps — a max-heap for left half and a min-heap for right half — to efficiently track median or partition data.

</div>

---

## 🧠 What is Two Heaps?

Use a **max-heap** (for smaller half) and **min-heap** (for larger half) to dynamically maintain statistics like median, or to efficiently partition data.

---

## 🔑 When to Use It?

```
✅ Find median of stream
✅ Sliding window median
✅ Reorganize string (max frequency)
✅ Top K frequent elements
✅ Task scheduler
✅ Rearrange string with duplicates
```

---

## 📐 ASCII Visualization

```
Stream: 1, 3, 2, 7, 5

After each add:
1:       [1]  |
1,3:     [1]  | [3]       → median = 2
1,3,2:   [2,1]| [3]       → median = 2
1,3,2,7: [2,1]| [3,7]     → median = 2.5
1,3,2,7,5: [3,2,1]| [5,7]  → median = 3
```

---

## 📋 Template Code

### Find Median from Data Stream
```java
class MedianFinder {
    PriorityQueue<Integer> maxHeap;  // left half, max-heap
    PriorityQueue<Integer> minHeap;  // right half, min-heap
    
    public MedianFinder() {
        maxHeap = new PriorityQueue<>((a, b) -> b - a);  // max-heap
        minHeap = new PriorityQueue<>();                  // min-heap
    }
    
    public void addNum(int num) {
        maxHeap.offer(num);
        
        // Ensure every element in maxHeap <= every element in minHeap
        if (!maxHeap.isEmpty() && !minHeap.isEmpty() && 
            maxHeap.peek() > minHeap.peek()) {
            int temp = maxHeap.poll();
            maxHeap.offer(minHeap.poll());
            minHeap.offer(temp);
        }
        
        // Balance sizes: maxHeap size == minHeap size or maxHeap size = minHeap size + 1
        if (maxHeap.size() > minHeap.size() + 1) {
            minHeap.offer(maxHeap.poll());
        }
        if (minHeap.size() > maxHeap.size()) {
            maxHeap.offer(minHeap.poll());
        }
    }
    
    public double findMedian() {
        if (maxHeap.size() > minHeap.size()) {
            return maxHeap.peek();
        }
        return (maxHeap.peek() + minHeap.peek()) / 2.0;
    }
}
```

### Top K Frequent Elements
```java
public int[] topKFrequent(int[] nums, int k) {
    Map<Integer, Integer> freqMap = new HashMap<>();
    for (int num : nums) {
        freqMap.put(num, freqMap.getOrDefault(num, 0) + 1);
    }
    
    PriorityQueue<Integer> minHeap = new PriorityQueue<>(
        (a, b) -> freqMap.get(a) - freqMap.get(b)
    );
    
    for (int num : freqMap.keySet()) {
        minHeap.offer(num);
        if (minHeap.size() > k) {
            minHeap.poll();  // remove least frequent
        }
    }
    
    int[] result = new int[k];
    int idx = 0;
    while (!minHeap.isEmpty()) {
        result[idx++] = minHeap.poll();
    }
    return result;
}
```

---

## 🔬 Detailed Example — Reorganize String

**Problem:** Given string with char frequencies, return reorganized string with no adjacent duplicates.

**Input:** `"aab"` → **Output:** `"aba"` or `"baa"`

```java
public String reorganizeString(String s) {
    // Count frequencies
    Map<Character, Integer> freqMap = new HashMap<>();
    for (char c : s.toCharArray()) {
        freqMap.put(c, freqMap.getOrDefault(c, 0) + 1);
    }
    
    // Max heap by frequency
    PriorityQueue<Character> maxHeap = new PriorityQueue<>(
        (a, b) -> freqMap.get(b) - freqMap.get(a)
    );
    maxHeap.addAll(freqMap.keySet());
    
    StringBuilder result = new StringBuilder();
    while (!maxHeap.isEmpty()) {
        char first = maxHeap.poll();
        if (result.isEmpty() || result.charAt(result.length() - 1) != first) {
            result.append(first);
            freqMap.put(first, freqMap.get(first) - 1);
            if (freqMap.get(first) > 0) {
                maxHeap.offer(first);
            }
        } else if (!maxHeap.isEmpty()) {
            char second = maxHeap.poll();
            result.append(second);
            freqMap.put(second, freqMap.get(second) - 1);
            if (freqMap.get(second) > 0) {
                maxHeap.offer(second);
            }
            maxHeap.offer(first);
        } else {
            return "";  // impossible
        }
    }
    
    return result.toString();
}

// Trace for "aab":
// Frequencies: a=2, b=1
// maxHeap: [a(2), b(1)]
// Pick a: result="a", a→1
// Pick b: result="ab", b→0
// Pick a: result="aba", a→0
```

---

## 💡 Key Insight

> **Two-heap balance.** Max-heap ≤ Min-heap always. Sizes differ by ≤1. For K frequent, keep K-sized min-heap of frequencies.

---

## 🧩 Common Variations

| Variation | Approach |
|-----------|----------|
| Median stream | Balance both heaps |
| Top K frequent | Min-heap of size K |
| Reorganize | Max-heap, greedy pick |
| Task scheduler | Simulate with heaps |

---

## 🏋️ Practice Problems

### 🟡 Medium
| # | Problem | Platform | Key Trick |
|---|---------|----------|-----------|
| 1 | [Find Median Data Stream](https://leetcode.com/problems/find-median-from-data-stream/) | LeetCode #295 | Two heaps |
| 2 | [Top K Frequent Elements](https://leetcode.com/problems/top-k-frequent-elements/) | LeetCode #347 | Min-heap size K |
| 3 | [Reorganize String](https://leetcode.com/problems/reorganize-string/) | LeetCode #767 | Max-heap greedy |
| 4 | [Task Scheduler](https://leetcode.com/problems/task-scheduler/) | LeetCode #621 | Greedy + heap |
| 5 | [Rearrange String K Dist Apart](https://leetcode.com/problems/rearrange-string-k-distance-apart/) | LeetCode #358 | Max-heap + cooldown |

### 🔴 Hard
| # | Problem | Platform | Key Trick |
|---|---------|----------|-----------|
| 6 | [Sliding Window Median](https://leetcode.com/problems/sliding-window-median/) | LeetCode #480 | Two heaps + lazy removal |

---

## 📊 Complexity

| Operation | Time | Space |
|-----------|------|-------|
| Add median | O(log n) | O(n) |
| Top K | O(n log k) | O(k) |
| Reorganize | O(n log k) where k=unique | O(k) |

---

## 🔗 Navigation

[← Tree DFS](./08-tree-dfs.md) | [Back to Main](../README.md) | [Next: Subsets →](./10-subsets.md)
