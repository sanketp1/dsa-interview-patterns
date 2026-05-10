<div align="center">

# 🔀 Pattern 04 — Merge Intervals

[![Difficulty](https://img.shields.io/badge/Difficulty-Medium-yellow?style=flat-square)](#)
[![Problems](https://img.shields.io/badge/Problems-10+-blue?style=flat-square)](#practice-problems)

> **Core Idea:** Sort intervals by start time, then merge overlapping ones in a single pass.

</div>

---

## 🧠 What is the Merge Intervals Pattern?

Given a list of intervals (start, end pairs), find how they overlap, merge them, or insert new ones.

Key insight: **sort by start time** first, then it's a linear scan.

---

## 🔑 When to Use It?

```
✅ "Given a list of intervals, merge overlapping ones"
✅ "Find if any two meetings conflict"
✅ "Insert an interval into a sorted list"
✅ "Minimum number of meeting rooms"
✅ Schedule / calendar problems
✅ "Find free time slots"
```

---

## 📐 ASCII Visualization

```
Input:  [[1,3],[2,6],[8,10],[15,18]]

Timeline:
  1---3           (interval 1)
    2------6      (interval 2, overlaps!)
              8--10    (no overlap)
                      15---18  (no overlap)

After merge:
  1------6    8--10    15---18

Output: [[1,6],[8,10],[15,18]]
```

---

## 🧩 Overlap Logic

```
Two intervals [a,b] and [c,d] OVERLAP if:
    c <= b  (new start before current end)

How to merge:
    new_end = max(b, d)
    merged = [a, new_end]
```

---

## 📋 Template Code

### Merge Overlapping Intervals
```java
public int[][] merge(int[][] intervals) {
    if (intervals == null || intervals.length == 0) {
        return new int[0][0];
    }

    Arrays.sort(intervals, (a, b) -> Integer.compare(a[0], b[0]));  // sort by start
    List<int[]> merged = new ArrayList<>();
    merged.add(intervals[0]);

    for (int i = 1; i < intervals.length; i++) {
        int start = intervals[i][0];
        int end = intervals[i][1];
        int lastEnd = merged.get(merged.size() - 1)[1];

        if (start <= lastEnd) {            // overlapping
            merged.get(merged.size() - 1)[1] = Math.max(lastEnd, end);
        } else {                           // gap, new interval
            merged.add(new int[] {start, end});
        }
    }

    return merged.toArray(new int[0][0]);
}
```

### Insert Interval
```java
public int[][] insert(int[][] intervals, int[] newInterval) {
    List<int[]> result = new ArrayList<>();
    int i = 0;
    int n = intervals.length;

    // Add all intervals that end before new starts
    while (i < n && intervals[i][1] < newInterval[0]) {
        result.add(intervals[i]);
        i++;
    }

    // Merge all overlapping intervals
    while (i < n && intervals[i][0] <= newInterval[1]) {
        newInterval[0] = Math.min(newInterval[0], intervals[i][0]);
        newInterval[1] = Math.max(newInterval[1], intervals[i][1]);
        i++;
    }

    result.add(newInterval);

    // Add remaining
    while (i < n) {
        result.add(intervals[i]);
        i++;
    }

    return result.toArray(new int[0][0]);
}
```

---

## 🔬 Detailed Example — Minimum Meeting Rooms

**Problem:** Given meeting time intervals, find the minimum number of conference rooms required.

**Input:** `[[0,30],[5,10],[15,20]]` → **Output:** `2`

```java
public int minMeetingRooms(int[][] intervals) {
    if (intervals == null || intervals.length == 0) {
        return 0;
    }

    Arrays.sort(intervals, (a, b) -> Integer.compare(a[0], b[0]));  // sort by start
    PriorityQueue<Integer> heap = new PriorityQueue<>();  // min-heap of end times

    for (int[] interval : intervals) {
        int start = interval[0];
        int end = interval[1];

        // If earliest ending meeting ended before this starts
        if (!heap.isEmpty() && heap.peek() <= start) {
            heap.poll();  // reuse the room
        }
        heap.offer(end);     // add/new room

    }

    return heap.size();
}

// Visual:
// Meeting 1: [0,30]  rooms=1  heap=[30]
// Meeting 2: [5,10]  5>30? No → new room  heap=[10,30]
// Meeting 3: [15,20] 15>10? Yes → reuse  heap=[20,30]
// Answer: 2 rooms
```

---

## 💡 Key Insight

> **Sort → Scan → Track.** Sorting by start time ensures you always compare a new interval to the last merged one. For "how many rooms", a min-heap tracks the earliest available room.

---

## 🏋️ Practice Problems

### 🟡 Medium
| # | Problem | Platform | Key Trick |
|---|---------|----------|-----------|
| 1 | [Merge Intervals](https://leetcode.com/problems/merge-intervals/) | LeetCode #56 | Sort + merge pass |
| 2 | [Insert Interval](https://leetcode.com/problems/insert-interval/) | LeetCode #57 | 3-phase insert |
| 3 | [Non-overlapping Intervals](https://leetcode.com/problems/non-overlapping-intervals/) | LeetCode #435 | Sort by end, greedy |
| 4 | [Meeting Rooms](https://leetcode.com/problems/meeting-rooms/) | LeetCode #252 | Sort + check adjacent |
| 5 | [Meeting Rooms II](https://leetcode.com/problems/meeting-rooms-ii/) | LeetCode #253 | Min-heap of ends |
| 6 | [Interval List Intersections](https://leetcode.com/problems/interval-list-intersections/) | LeetCode #986 | Two pointers on two lists |
| 7 | [Employee Free Time](https://leetcode.com/problems/employee-free-time/) | LeetCode #759 | Flatten + find gaps |

### 🔴 Hard
| # | Problem | Platform | Key Trick |
|---|---------|----------|-----------|
| 8 | [Minimum Number of Arrows to Burst Balloons](https://leetcode.com/problems/minimum-number-of-arrows-to-burst-balloons/) | LeetCode #452 | Sort by end |
| 9 | [Data Stream as Disjoint Intervals](https://leetcode.com/problems/data-stream-as-disjoint-intervals/) | LeetCode #352 | Insert + merge on the fly |

---

## 📊 Complexity

| Operation | Time | Space |
|-----------|------|-------|
| Merge | O(n log n) — sort dominates | O(n) |
| Insert | O(n) | O(n) |
| Meeting Rooms | O(n log n) | O(n) — heap |

---

## 🔗 Navigation

[← Fast & Slow Pointers](./03-fast-slow-pointers.md) | [Back to Main](../README.md) | [Next: Cyclic Sort →](./05-cyclic-sort.md)