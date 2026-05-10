<div align="center">

# 📊 Pattern 21 — Monotonic Stack

[![Difficulty](https://img.shields.io/badge/Difficulty-Medium-yellow?style=flat-square)](#)
[![Problems](https://img.shields.io/badge/Problems-10+-blue?style=flat-square)](#practice-problems)

> **Core Idea:** Maintain a stack in increasing or decreasing order. Pop when constraint violated; useful for "next greater/smaller" problems.

</div>

---

## 🧠 What is Monotonic Stack?

Use a **stack maintaining order** to efficiently find next/previous greater/smaller elements. Instead of O(n²) brute force, achieve O(n).

---

## 🔑 When to Use It?

```
✅ Next greater element
✅ Next smaller element
✅ Daily temperatures
✅ Largest rectangle in histogram
✅ Trapping rain water
✅ Stock span
✅ Remove K digits
```

---

## 📐 Visual Concept

```
Array: [2, 1, 2, 4, 3]

Next Greater:
2 → 4
1 → 2
2 → 4
4 → -1
3 → -1

Stack (decreasing):
Process 2: [2]
Process 1: [2, 1]
Process 2: pop 1 (2>1, answer[1]=2), [2, 2]
Process 4: pop 2,2 (4>2, answer[2]=4, answer[0]=4), [4]
Process 3: [4, 3]
```

---

## 📋 Template Code

### Next Greater Element
```java
public int[] nextGreaterElement(int[] nums) {
    int n = nums.length;
    int[] result = new int[n];
    Arrays.fill(result, -1);
    
    Stack<Integer> stack = new Stack<>();  // maintains indices
    
    for (int i = n - 1; i >= 0; i--) {
        // Pop smaller elements
        while (!stack.isEmpty() && nums[stack.peek()] <= nums[i]) {
            stack.pop();
        }
        
        // Top is next greater (or empty)
        if (!stack.isEmpty()) {
            result[i] = nums[stack.peek()];
        }
        
        stack.push(i);
    }
    
    return result;
}
```

### Daily Temperatures (Next Greater Index)
```java
public int[] dailyTemperatures(int[] temperatures) {
    int n = temperatures.length;
    int[] result = new int[n];
    Stack<Integer> stack = new Stack<>();  // maintains indices in decreasing order
    
    for (int i = 0; i < n; i++) {
        // Pop warmer days
        while (!stack.isEmpty() && temperatures[stack.peek()] < temperatures[i]) {
            int prevIndex = stack.pop();
            result[prevIndex] = i - prevIndex;
        }
        
        stack.push(i);
    }
    
    return result;
}
```

---

## 🔬 Detailed Example — Largest Rectangle in Histogram

**Problem:** Find max area of rectangle in histogram.

**Input:** `[2,1,5,6,2,3]` → **Output:** `10` (width=2, height=5)

```java
public int largestRectangleArea(int[] heights) {
    Stack<Integer> stack = new Stack<>();  // maintains indices in increasing height order
    int maxArea = 0;
    
    for (int i = 0; i < heights.length; i++) {
        // Pop taller bars
        while (!stack.isEmpty() && heights[stack.peek()] > heights[i]) {
            int h = heights[stack.pop()];
            int w = stack.isEmpty() ? i : i - stack.peek() - 1;
            maxArea = Math.max(maxArea, h * w);
        }
        
        stack.push(i);
    }
    
    // Pop remaining
    while (!stack.isEmpty()) {
        int h = heights[stack.pop()];
        int w = stack.isEmpty() ? heights.length : heights.length - stack.peek() - 1;
        maxArea = Math.max(maxArea, h * w);
    }
    
    return maxArea;
}

// Trace for [2,1,5,6,2,3]:
// i=0: push 0, stack=[0]
// i=1: heights[0]=2 > heights[1]=1, pop 0
//      area = 2 * 1 = 2, maxArea=2, push 1, stack=[1]
// i=2: heights[1]=1 < heights[2]=5, push 2, stack=[1,2]
// i=3: heights[2]=5 < heights[3]=6, push 3, stack=[1,2,3]
// i=4: heights[3]=6 > heights[4]=2, pop 3
//      area = 6 * 1 = 6, maxArea=6
//      pop 2: area = 5 * 2 = 10, maxArea=10
//      push 4, stack=[1,4]
// i=5: heights[4]=2 < heights[5]=3, push 5, stack=[1,4,5]
// Final pops: area = 3 * 1 = 3, area = 2 * 4 = 8
// Result: maxArea=10
```

---

## 💡 Key Insight

> **Maintain order, pop violators.** When pushing, pop anything that breaks the order. Popped elements have found their "next"; top of stack is potential answer.

---

## 🧩 Common Variations

| Problem | Stack Order |
|---------|------------|
| Next greater | Decreasing |
| Next smaller | Increasing |
| Largest rectangle | Increasing heights |
| Trapping rain water | Decreasing |
| Remove K digits | Increasing |

---

## 🏋️ Practice Problems

### 🟢 Easy
| # | Problem | Platform | Key Trick |
|---|---------|----------|-----------|
| 1 | [Next Greater Element I](https://leetcode.com/problems/next-greater-element-i/) | LeetCode #496 | Decreasing stack |

### 🟡 Medium
| # | Problem | Platform | Key Trick |
|---|---------|----------|-----------|
| 2 | [Daily Temperatures](https://leetcode.com/problems/daily-temperatures/) | LeetCode #739 | Distance = index diff |
| 3 | [Largest Rectangle Histogram](https://leetcode.com/problems/largest-rectangle-in-histogram/) | LeetCode #84 | Width calc |
| 4 | [Trapping Rain Water](https://leetcode.com/problems/trapping-rain-water/) | LeetCode #42 | Decreasing stack |
| 5 | [Remove K Digits](https://leetcode.com/problems/remove-k-digits-to-make-smallest-number/) | LeetCode #402 | Increasing stack |
| 6 | [Stock Span](https://leetcode.com/problems/online-stock-span/) | LeetCode #901 | Span logic |

### 🔴 Hard
| # | Problem | Platform | Key Trick |
|---|---------|----------|-----------|
| 7 | [Largest Rectangle II](https://leetcode.com/problems/maximal-rectangle/) | LeetCode #85 | Histogram per row |

---

## 📊 Complexity

| Operation | Time | Space |
|-----------|------|-------|
| Next greater | O(n) | O(n) stack |
| Daily temps | O(n) | O(n) |
| Largest rect | O(n) | O(n) |

---

## 🔗 Navigation

[← Graph Islands](./20-graph-islands.md) | [Back to Main](../README.md)
