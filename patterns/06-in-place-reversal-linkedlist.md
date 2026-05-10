<div align="center">

# 🔗 Pattern 06 — In-Place Reversal of Linked List

[![Difficulty](https://img.shields.io/badge/Difficulty-Medium-yellow?style=flat-square)](#)
[![Problems](https://img.shields.io/badge/Problems-10+-blue?style=flat-square)](#practice-problems)

> **Core Idea:** Reverse links while traversing; maintain three pointers: previous, current, next.

</div>

---

## 🧠 What is In-Place Reversal?

Reverse a linked list (or a portion of it) by changing the direction of links **without extra space**. Think of it as redirecting arrows in a chain.

---

## 🔑 When to Use It?

```
✅ "Reverse a linked list"
✅ "Reverse first k nodes"
✅ "Reverse between two positions"
✅ "Reverse alternate nodes"
✅ Palindrome linked list
✅ Reorder list (reverse second half)
```

---

## 📐 ASCII Visualization

```
Original:  1 → 2 → 3 → 4 → null

Step 1:    null ← 1   2 → 3 → 4 → null
           (prev) (curr)(next group)

Step 2:    null ← 1 ← 2   3 → 4 → null

Step 3:    null ← 1 ← 2 ← 3   4 → null

Step 4:    null ← 1 ← 2 ← 3 ← 4

Result:    4 → 3 → 2 → 1 → null
```

---

## 📋 Template Code

### Reverse Entire Linked List
```java
public ListNode reverseList(ListNode head) {
    ListNode prev = null;
    ListNode curr = head;
    
    while (curr != null) {
        ListNode nextTemp = curr.next;  // save next node
        curr.next = prev;               // reverse the link
        prev = curr;                    // move prev forward
        curr = nextTemp;                // move curr forward
    }
    
    return prev;  // new head
}
```

### Reverse Between Two Positions
```java
public ListNode reverseBetween(ListNode head, int left, int right) {
    if (left == right) return head;
    
    ListNode dummy = new ListNode(0);
    dummy.next = head;
    ListNode prevLeft = dummy;
    ListNode curr = head;
    
    // Move to node before 'left' position
    for (int i = 0; i < left - 1; i++) {
        prevLeft = curr;
        curr = curr.next;
    }
    
    ListNode prev = null;
    // Reverse 'right - left + 1' nodes
    for (int i = 0; i < right - left + 1; i++) {
        ListNode nextTemp = curr.next;
        curr.next = prev;
        prev = curr;
        curr = nextTemp;
    }
    
    prevLeft.next.next = curr;  // connect reversed end to remaining list
    prevLeft.next = prev;        // connect start to reversed part
    
    return dummy.next;
}
```

---

## 🔬 Detailed Example — Palindrome Linked List

**Problem:** Check if a linked list is a palindrome.

**Input:** `1 → 2 → 2 → 1` → **Output:** `true`

```java
public boolean isPalindrome(ListNode head) {
    if (head == null || head.next == null) return true;
    
    // Step 1: Find middle
    ListNode slow = head, fast = head;
    while (fast != null && fast.next != null) {
        slow = slow.next;
        fast = fast.next.next;
    }
    
    // Step 2: Reverse second half
    ListNode secondHalf = reverseList(slow);
    
    // Step 3: Compare
    ListNode p1 = head, p2 = secondHalf;
    while (p2 != null) {  // second half is shorter for odd length
        if (p1.val != p2.val) return false;
        p1 = p1.next;
        p2 = p2.next;
    }
    
    return true;
}

private ListNode reverseList(ListNode head) {
    ListNode prev = null, curr = head;
    while (curr != null) {
        ListNode nextTemp = curr.next;
        curr.next = prev;
        prev = curr;
        curr = nextTemp;
    }
    return prev;
}

// Trace for 1 → 2 → 2 → 1:
// After finding middle: slow at second 2
// After reversing: 1 → 2 → null and 1 → 2 → null
// Compare: 1==1 ✓, 2==2 ✓ → palindrome!
```

---

## 💡 Key Insight

> **Three pointers, one pass.** Save next, reverse link, advance. Dummy node helps handle edge cases like reversing from head.

---

## 🧩 Common Variations

| Variation | Approach |
|-----------|----------|
| Full reversal | Iterate entire list |
| Partial reversal | Bookmark positions, reverse middle |
| K-Group reversal | Group of K, reverse, link groups |
| Palindrome check | Find middle + reverse + compare |

---

## 🏋️ Practice Problems

### 🟢 Easy
| # | Problem | Platform | Key Trick |
|---|---------|----------|-----------|
| 1 | [Reverse Linked List](https://leetcode.com/problems/reverse-linked-list/) | LeetCode #206 | Basic reversal |
| 2 | [Palindrome Linked List](https://leetcode.com/problems/palindrome-linked-list/) | LeetCode #234 | Reverse second half |

### 🟡 Medium
| # | Problem | Platform | Key Trick |
|---|---------|----------|-----------|
| 3 | [Reverse Linked List II](https://leetcode.com/problems/reverse-linked-list-ii/) | LeetCode #92 | Reverse between positions |
| 4 | [Reverse Nodes in K-Group](https://leetcode.com/problems/reverse-nodes-in-k-group/) | LeetCode #25 | Reverse K-sized groups |
| 5 | [Reorder List](https://leetcode.com/problems/reorder-list/) | LeetCode #143 | Find mid + reverse + merge |
| 6 | [Odd Even Linked List](https://leetcode.com/problems/odd-even-linked-list/) | LeetCode #328 | Partition + optional reverse |

### 🔴 Hard
| # | Problem | Platform | Key Trick |
|---|---------|----------|-----------|
| 7 | [Reverse Nodes in K-Group](https://leetcode.com/problems/reverse-nodes-in-k-group/) | LeetCode #25 | Recursive or iterative |

---

## 📊 Complexity

| Operation | Time | Space |
|-----------|------|-------|
| Reverse list | O(n) | O(1) |
| Reverse between | O(n) | O(1) |
| Palindrome check | O(n) | O(1) |

---

## 🔗 Navigation

[← Cyclic Sort](./05-cyclic-sort.md) | [Back to Main](../README.md) | [Next: Tree BFS →](./07-tree-bfs.md)
