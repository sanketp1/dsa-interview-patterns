<div align="center">

# 🔀 Pattern 14 — K-Way Merge

[![Difficulty](https://img.shields.io/badge/Difficulty-Medium-yellow?style=flat-square)](#)
[![Problems](https://img.shields.io/badge/Problems-8+-blue?style=flat-square)](#practice-problems)

> **Core Idea:** Merge K sorted lists/arrays using a min-heap to always pick the smallest next element.

</div>

---

## 🧠 What is K-Way Merge?

Efficiently merge K sorted data structures by maintaining a **min-heap of size K** (one pointer per list) and always extracting the smallest element.

---

## 🔑 When to Use It?

```
✅ Merge K sorted lists
✅ Merge K sorted arrays
✅ Find median from sorted arrays
✅ Smallest range covering from K lists
✅ Merge sorted subarrays
```

---

## 📐 ASCII Visualization

```
List 1: [1, 4, 5]
List 2: [1, 3, 4]
List 3: [2, 6]

Heap (pointers): [1(L1), 1(L2), 2(L3)]
                  ↓
                [1(L1), 1(L2), 2(L3)]
                Poll 1, push 4(L1)
                [1(L2), 2(L3), 4(L1)]
                ...
Result: [1,1,1,2,3,4,4,5,6]
```

---

## 📋 Template Code

### Merge K Sorted Lists
```java
public ListNode mergeKLists(ListNode[] lists) {
    if (lists == null || lists.length == 0) return null;
    
    PriorityQueue<ListNode> minHeap = new PriorityQueue<>(
        (a, b) -> Integer.compare(a.val, b.val)
    );
    
    // Add first node of each list
    for (ListNode list : lists) {
        if (list != null) {
            minHeap.offer(list);
        }
    }
    
    ListNode dummy = new ListNode(0);
    ListNode current = dummy;
    
    while (!minHeap.isEmpty()) {
        ListNode smallest = minHeap.poll();
        current.next = smallest;
        current = current.next;
        
        if (smallest.next != null) {
            minHeap.offer(smallest.next);
        }
    }
    
    return dummy.next;
}
```

### Merge K Sorted Arrays
```java
public int[] mergeKArrays(int[][] arrays) {
    // Min-heap: [value, array_index, element_index]
    PriorityQueue<int[]> minHeap = new PriorityQueue<>(
        (a, b) -> Integer.compare(a[0], b[0])
    );
    
    int totalSize = 0;
    
    // Add first element of each array
    for (int i = 0; i < arrays.length; i++) {
        if (arrays[i].length > 0) {
            minHeap.offer(new int[] {arrays[i][0], i, 0});
            totalSize += arrays[i].length;
        }
    }
    
    int[] result = new int[totalSize];
    int idx = 0;
    
    while (!minHeap.isEmpty()) {
        int[] current = minHeap.poll();
        int value = current[0];
        int arrayIdx = current[1];
        int elementIdx = current[2];
        
        result[idx++] = value;
        
        // Add next element from same array
        if (elementIdx + 1 < arrays[arrayIdx].length) {
            minHeap.offer(new int[] {arrays[arrayIdx][elementIdx + 1], arrayIdx, elementIdx + 1});
        }
    }
    
    return result;
}
```

---

## 🔬 Detailed Example — Smallest Range Covering

**Problem:** Find smallest range covering at least one number from each of K lists.

**Input:** `[[4,10,15,24,26],[0,9,12,20],[5,18,22,30]]` → **Output:** `[20,24]`

```java
public int[] smallestRange(int[][] nums) {
    PriorityQueue<int[]> minHeap = new PriorityQueue<>(
        (a, b) -> Integer.compare(a[0], b[0])
    );
    
    int maxVal = Integer.MIN_VALUE;
    int rangeStart = 0, rangeEnd = Integer.MAX_VALUE;
    
    // Add first element of each list
    for (int i = 0; i < nums.length; i++) {
        minHeap.offer(new int[] {nums[i][0], i, 0});  // [value, list_idx, elem_idx]
        maxVal = Math.max(maxVal, nums[i][0]);
    }
    
    while (minHeap.size() == nums.length) {
        int[] current = minHeap.poll();
        int minVal = current[0];
        int listIdx = current[1];
        int elemIdx = current[2];
        
        if (maxVal - minVal < rangeEnd - rangeStart) {
            rangeStart = minVal;
            rangeEnd = maxVal;
        }
        
        if (elemIdx + 1 < nums[listIdx].length) {
            int nextVal = nums[listIdx][elemIdx + 1];
            minHeap.offer(new int[] {nextVal, listIdx, elemIdx + 1});
            maxVal = Math.max(maxVal, nextVal);
        }
    }
    
    return new int[] {rangeStart, rangeEnd};
}

// Trace for [[4,10,15,24,26],[0,9,12,20],[5,18,22,30]]:
// Start: heap=[4(L0), 0(L1), 5(L2)], max=5, range=[0,5]
// Poll 0, push 9: heap=[4(L0), 5(L2), 9(L1)], max=9, range=[4,9]
// ...continue until we find [20,24]
```

---

## 💡 Key Insight

> **Min-heap pointer for each list.** Always merge the smallest current element from any list, then advance that list's pointer.

---

## 🧩 Common Variations

| Variation | Approach |
|-----------|----------|
| Merge lists | Heap of ListNode pointers |
| Merge arrays | Heap of [value, array_idx, elem_idx] |
| Smallest range | Heap + track max |
| Median of sorted | Two heaps on merged |

---

## 🏋️ Practice Problems

### 🟡 Medium
| # | Problem | Platform | Key Trick |
|---|---------|----------|-----------|
| 1 | [Merge K Sorted Lists](https://leetcode.com/problems/merge-k-sorted-lists/) | LeetCode #23 | Heap pointers |
| 2 | [Smallest Range](https://leetcode.com/problems/smallest-range-covering-elements-from-k-lists/) | LeetCode #632 | Track max + merge |
| 3 | [Merge Sorted Array](https://leetcode.com/problems/merge-sorted-array/) | LeetCode #88 | Two pointers (K=2) |

### 🔴 Hard
| # | Problem | Platform | Key Trick |
|---|---------|----------|-----------|
| 4 | [Merge K Sorted Lists](https://leetcode.com/problems/merge-k-sorted-lists/) | LeetCode #23 | Heap at scale |

---

## 📊 Complexity

| Operation | Time | Space |
|-----------|------|-------|
| Merge K lists | O(n log k) where n=total | O(k) heap |
| Merge K arrays | O(n log k) | O(k) heap |
| Smallest range | O(n log k) | O(k) heap |

---

## 🔗 Navigation

[← Top K Elements](./13-top-k-elements.md) | [Back to Main](../README.md) | [Next: 0/1 Knapsack →](./15-01-knapsack-dp.md)
