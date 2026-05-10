<div align="center">

# 👑 Pattern 13 — Top K Elements

[![Difficulty](https://img.shields.io/badge/Difficulty-Medium-yellow?style=flat-square)](#)
[![Problems](https://img.shields.io/badge/Problems-10+-blue?style=flat-square)](#practice-problems)

> **Core Idea:** Use a min-heap of size K to efficiently track K largest/most-frequent elements.

</div>

---

## 🧠 What is Top K Elements?

Efficiently find K largest numbers, K most frequent elements, or similar "top K" queries. Use a **min-heap** bounded to size K.

---

## 🔑 When to Use It?

```
✅ Top K largest numbers
✅ K most frequent elements
✅ Kth largest element
✅ Kth smallest element
✅ K closest points to origin
✅ Reorganize string with K distance
```

---

## 📐 ASCII Visualization

```
Array: [3,2,1,5,6,4], K=2

Min-heap (size 2):
Maintain: [5, 6]

At end: 5, 6 are top 2
```

---

## 📋 Template Code

### Kth Largest Element
```java
public int findKthLargest(int[] nums, int k) {
    PriorityQueue<Integer> minHeap = new PriorityQueue<>();
    
    for (int num : nums) {
        minHeap.offer(num);
        if (minHeap.size() > k) {
            minHeap.poll();  // remove smallest when > k
        }
    }
    
    return minHeap.peek();  // Kth largest at top
}
```

### K Closest Points to Origin
```java
public int[][] kClosest(int[][] points, int k) {
    PriorityQueue<int[]> maxHeap = new PriorityQueue<>(
        (a, b) -> (b[0] * b[0] + b[1] * b[1]) - (a[0] * a[0] + a[1] * a[1])
    );
    
    for (int[] point : points) {
        maxHeap.offer(point);
        if (maxHeap.size() > k) {
            maxHeap.poll();
        }
    }
    
    return maxHeap.toArray(new int[k][2]);
}
```

---

## 🔬 Detailed Example — K Most Frequent Words

**Problem:** Return K most frequent words in order of frequency (ties: lexicographic).

**Input:** `["apple","banana","apple","cherry","banana","apple"]`, k=2 → **Output:** `["apple","banana"]`

```java
public List<String> topKFrequent(String[] words, int k) {
    // Step 1: Count frequencies
    Map<String, Integer> freqMap = new HashMap<>();
    for (String word : words) {
        freqMap.put(word, freqMap.getOrDefault(word, 0) + 1);
    }
    
    // Step 2: Min-heap by frequency (then max-heap by lexicographic)
    PriorityQueue<String> minHeap = new PriorityQueue<>(
        (a, b) -> {
            int freqDiff = freqMap.get(a) - freqMap.get(b);
            if (freqDiff != 0) return freqDiff;  // lower freq comes out first
            return b.compareTo(a);  // if same freq, lex higher first → max-heap behavior
        }
    );
    
    // Step 3: Maintain heap of size k
    for (String word : freqMap.keySet()) {
        minHeap.offer(word);
        if (minHeap.size() > k) {
            minHeap.poll();
        }
    }
    
    // Step 4: Extract result (reverse for correct order)
    List<String> result = new ArrayList<>();
    while (!minHeap.isEmpty()) {
        result.add(minHeap.poll());
    }
    Collections.reverse(result);
    return result;
}

// Trace for ["apple","banana","apple","cherry","banana","apple"], k=2:
// Frequencies: apple=3, banana=2, cherry=1
// Heap: [banana(2), apple(3)] → maintain size 2
// Poll & reverse: [apple, banana]
```

---

## 💡 Key Insight

> **Min-heap of size K keeps top K.** Every time we add an element > K size, we remove the smallest (worst element). Final heap contains K best elements.

---

## 🧩 Common Variations

| Variation | Approach |
|-----------|----------|
| K largest | Min-heap size K |
| K most frequent | Min-heap by frequency |
| K closest | Min-heap by distance |
| Kth element | Min-heap top K |

---

## 🏋️ Practice Problems

### 🟡 Medium
| # | Problem | Platform | Key Trick |
|---|---------|----------|-----------|
| 1 | [Kth Largest Element](https://leetcode.com/problems/kth-largest-element-in-an-array/) | LeetCode #215 | Min-heap size K |
| 2 | [Top K Frequent](https://leetcode.com/problems/top-k-frequent-elements/) | LeetCode #347 | Freq + heap |
| 3 | [K Closest Points](https://leetcode.com/problems/k-closest-points-to-origin/) | LeetCode #973 | Distance + heap |
| 4 | [Top K Frequent Words](https://leetcode.com/problems/top-k-frequent-words/) | LeetCode #692 | Freq + lexicographic |
| 5 | [Reorganize String](https://leetcode.com/problems/reorganize-string/) | LeetCode #767 | Max-heap variant |

---

## 📊 Complexity

| Operation | Time | Space |
|-----------|------|-------|
| Kth Largest | O(n log k) | O(k) |
| K Most Frequent | O(n log k) | O(n) + O(k) |
| K Closest | O(n log k) | O(k) |

---

## 🔗 Navigation

[← Bitwise XOR](./12-bitwise-xor.md) | [Back to Main](../README.md) | [Next: K-Way Merge →](./14-k-way-merge.md)
