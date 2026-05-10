<div align="center">

# 🐢 Pattern 03 — Fast & Slow Pointers (Floyd's Algorithm)

[![Difficulty](https://img.shields.io/badge/Difficulty-Medium-yellow?style=flat-square)](#)
[![Problems](https://img.shields.io/badge/Problems-10+-blue?style=flat-square)](#practice-problems)

> **Core Idea:** Two pointers at different speeds — the fast one laps the slow one if a cycle exists.

</div>

---

## 🧠 What is the Fast & Slow Pointer Pattern?

Also known as **Floyd's Tortoise and Hare**, this uses two pointers:
- 🐢 **Slow** moves 1 step at a time
- 🐇 **Fast** moves 2 steps at a time

If there's a **cycle**, fast eventually catches slow. If not, fast reaches the end.

---

## 🔑 When to Use It?

```
✅ Detect a cycle in a linked list
✅ Find the start of a cycle
✅ Find the middle of a linked list
✅ Detect a cycle in an array (repeated jumps)
✅ Happy number problem
✅ Find duplicate number (array as implicit linked list)
```

---

## 📐 ASCII Visualization

```
Linked List with Cycle:
1 → 2 → 3 → 4 → 5
            ↑       ↓
            8 ← 7 ← 6

Start:  Slow=1, Fast=1
Step 1: Slow=2, Fast=3
Step 2: Slow=3, Fast=5
Step 3: Slow=4, Fast=7
Step 4: Slow=5, Fast=4
Step 5: Slow=6, Fast=6  ← MEET! Cycle detected.
```

---

## 📋 Template Code

### Cycle Detection
```java
public boolean hasCycle(ListNode head) {
    ListNode slow = head, fast = head;

    while (fast != null && fast.next != null) {
        slow = slow.next;
        fast = fast.next.next;

        if (slow == fast) {
            return true;   // cycle found
        }
    }

    return false;
}
```

### Find Cycle Start
```java
public ListNode findCycleStart(ListNode head) {
    ListNode slow = head, fast = head;

    // Phase 1: Detect cycle
    while (fast != null && fast.next != null) {
        slow = slow.next;
        fast = fast.next.next;
        if (slow == fast) {
            break;
        }
    }
    
    if (fast == null || fast.next == null) {
        return null;  // no cycle
    }

    // Phase 2: Find start
    // Reset one pointer to head; both move 1 step
    slow = head;
    while (slow != fast) {
        slow = slow.next;
        fast = fast.next;
    }

    return slow;   // start of cycle
}
```

### Find Middle of Linked List
```java
public ListNode findMiddle(ListNode head) {
    ListNode slow = head, fast = head;

    while (fast != null && fast.next != null) {
        slow = slow.next;
        fast = fast.next.next;
    }

    return slow;   // slow is at middle
}
```

---

## 🔬 Detailed Example — Happy Number

**Problem:** Determine if a number is "happy" (repeatedly summing digit squares reaches 1).

**Input:** `n = 19` → **Output:** `True`

```
19 → 1² + 9² = 82
82 → 8² + 2² = 68
68 → 6² + 8² = 100
100 → 1² + 0² + 0² = 1 ✅
```

```java
public boolean isHappy(int n) {
    int slow = n, fast = n;

    do {
        slow = digitSquareSum(slow);             // 1 step
        fast = digitSquareSum(digitSquareSum(fast));  // 2 steps
    } while (slow != fast);

    return slow == 1;   // cycle at 1 → happy; else unhappy cycle
}

private int digitSquareSum(int num) {
    int total = 0;
    while (num > 0) {
        int digit = num % 10;
        total += digit * digit;
        num /= 10;
    }
    return total;
}

// Unhappy numbers fall into a cycle that doesn't include 1
```

---

## 🔬 Detailed Example — Find Duplicate Number

**Problem:** Array of n+1 integers in range [1,n]. Find the duplicate without extra space.

```java
public int findDuplicate(int[] nums) {
    // Treat array as implicit linked list: i → nums[i]
    int slow = nums[0], fast = nums[0];

    // Phase 1: Find intersection
    do {
        slow = nums[slow];
        fast = nums[nums[fast]];
    } while (slow != fast);

    // Phase 2: Find entrance to cycle
    slow = nums[0];
    while (slow != fast) {
        slow = nums[slow];
        fast = nums[fast];
    }

    return slow;   // duplicate number
}
```

---

## 💡 Key Insight — Why Does Phase 2 Work?

```
Let:
  F = distance from head to cycle start
  a = distance from cycle start to meeting point
  C = cycle length

At meeting point:
  slow traveled: F + a
  fast traveled: F + a + nC (lapped cycle n times)
  fast = 2 * slow → F + a + nC = 2(F + a)
  → F = nC - a

So from head and from meeting point,
both pointers reach cycle start at the same time! 🤯
```

---

## 🏋️ Practice Problems

### 🟢 Easy
| # | Problem | Platform | Key Trick |
|---|---------|----------|-----------|
| 1 | [Linked List Cycle](https://leetcode.com/problems/linked-list-cycle/) | LeetCode #141 | Classic cycle detection |
| 2 | [Happy Number](https://leetcode.com/problems/happy-number/) | LeetCode #202 | Digit sum as linked list |
| 3 | [Middle of the Linked List](https://leetcode.com/problems/middle-of-the-linked-list/) | LeetCode #876 | Fast reaches end |
| 4 | [Palindrome Linked List](https://leetcode.com/problems/palindrome-linked-list/) | LeetCode #234 | Find mid + reverse second half |

### 🟡 Medium
| # | Problem | Platform | Key Trick |
|---|---------|----------|-----------|
| 5 | [Linked List Cycle II](https://leetcode.com/problems/linked-list-cycle-ii/) | LeetCode #142 | Phase 2 cycle start |
| 6 | [Find the Duplicate Number](https://leetcode.com/problems/find-the-duplicate-number/) | LeetCode #287 | Array as implicit list |
| 7 | [Reorder List](https://leetcode.com/problems/reorder-list/) | LeetCode #143 | Find mid + reverse + merge |
| 8 | [Rotate List](https://leetcode.com/problems/rotate-list/) | LeetCode #61 | Find cycle point |

### 🔴 Hard
| # | Problem | Platform | Key Trick |
|---|---------|----------|-----------|
| 9 | [Circular Array Loop](https://leetcode.com/problems/circular-array-loop/) | LeetCode #457 | Same direction fast/slow |

---

## 📊 Complexity

| Operation | Time | Space |
|-----------|------|-------|
| Cycle Detection | O(n) | O(1) |
| Find Cycle Start | O(n) | O(1) |
| Find Middle | O(n) | O(1) |

---

## 🔗 Navigation

[← Two Pointers](./02-two-pointers.md) | [Back to Main](../README.md) | [Next: Merge Intervals →](./04-merge-intervals.md)