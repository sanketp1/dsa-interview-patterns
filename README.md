<div align="center">

# 🧠 DSA Interview Patterns — Master Guide

> **Crack any coding interview by mastering patterns, not memorizing solutions.**

[![Patterns](https://img.shields.io/badge/Patterns-21-blueviolet?style=for-the-badge)](#patterns)
[![Questions](https://img.shields.io/badge/Practice%20Questions-200+-success?style=for-the-badge)](#)
[![Difficulty](https://img.shields.io/badge/Level-Beginner%20→%20Expert-orange?style=for-the-badge)](#)
[![PRs Welcome](https://img.shields.io/badge/PRs-Welcome-brightgreen?style=for-the-badge)](CONTRIBUTING.md)

<br/>

```
 Every hard LeetCode problem is just a combination of patterns you already know.
                         — Learn the pattern. Own the problem.
```

</div>

---

## 🗺️ Why Patterns?

Most people grind 300+ LeetCode problems randomly and still fail interviews.
The secret? **Top engineers think in patterns.**

When you see a new problem, you don't solve it from scratch —
you recognize *which pattern* it belongs to, apply the template, and adapt.

> 📌 **This repo gives you 21 battle-tested patterns** used to solve 90%+ of coding interview problems at FAANG and top product companies.

---

## 📚 The 21 Patterns

| # | Pattern | Core Idea | Difficulty |
|---|---------|-----------|------------|
| 01 | [🪟 Sliding Window](./patterns/01-sliding-window.md) | Expand/shrink a window over an array | 🟢 Easy–Medium |
| 02 | [👉 Two Pointers](./patterns/02-two-pointers.md) | Converge/diverge two indices | 🟢 Easy–Medium |
| 03 | [🐢 Fast & Slow Pointers](./patterns/03-fast-slow-pointers.md) | Detect cycles using speed difference | 🟡 Medium |
| 04 | [🔀 Merge Intervals](./patterns/04-merge-intervals.md) | Overlap/merge time ranges | 🟡 Medium |
| 05 | [🔁 Cyclic Sort](./patterns/05-cyclic-sort.md) | Place elements at their correct index | 🟡 Medium |
| 06 | [↩️ In-place Reversal of Linked List](./patterns/06-in-place-reversal-linkedlist.md) | Reverse links without extra space | 🟡 Medium |
| 07 | [🌊 Tree BFS](./patterns/07-tree-bfs.md) | Level-by-level tree traversal | 🟡 Medium |
| 08 | [🌲 Tree DFS](./patterns/08-tree-dfs.md) | Path-based tree traversal | 🟡 Medium |
| 09 | [⚖️ Two Heaps](./patterns/09-two-heaps.md) | Balance two halves via heaps | 🔴 Medium–Hard |
| 10 | [🧩 Subsets](./patterns/10-subsets.md) | Generate all combinations/permutations | 🟡 Medium |
| 11 | [🔍 Modified Binary Search](./patterns/11-modified-binary-search.md) | Binary search on non-trivial spaces | 🟡 Medium |
| 12 | [⚡ Bitwise XOR](./patterns/12-bitwise-xor.md) | Find unique/missing using XOR tricks | 🟢 Easy–Medium |
| 13 | [🏆 Top K Elements](./patterns/13-top-k-elements.md) | Find K largest/smallest efficiently | 🟡 Medium |
| 14 | [🔗 K-way Merge](./patterns/14-k-way-merge.md) | Merge K sorted structures | 🔴 Hard |
| 15 | [🎒 0/1 Knapsack (DP)](./patterns/15-01-knapsack-dp.md) | Classic DP with include/exclude choice | 🔴 Medium–Hard |
| 16 | [📊 Topological Sort](./patterns/16-topological-sort.md) | Order tasks respecting dependencies | 🔴 Medium–Hard |
| 17 | [🌀 Fibonacci DP](./patterns/17-fibonacci-dp.md) | Overlapping subproblems with DP | 🟡 Medium |
| 18 | [🔮 Palindromic Subsequence (DP)](./patterns/18-palindromic-subsequence-dp.md) | 2D DP on palindrome variants | 🔴 Hard |
| 19 | [📏 Longest Common Substring (DP)](./patterns/19-longest-common-substring-dp.md) | Compare two strings with 2D DP | 🔴 Hard |
| 20 | [🏝️ Graph Islands (BFS/DFS)](./patterns/20-graph-islands.md) | Explore connected components in grids | 🟡 Medium |
| 21 | [📈 Monotonic Stack](./patterns/21-monotonic-stack.md) | Maintain increasing/decreasing stack | 🔴 Medium–Hard |

---

## 🗂️ Pattern Categories

```
📦 Array & String
├── 01 Sliding Window
├── 02 Two Pointers
├── 12 Bitwise XOR
└── 21 Monotonic Stack

🔗 Linked List
├── 03 Fast & Slow Pointers
└── 06 In-place Reversal

🌳 Trees & Graphs
├── 07 Tree BFS
├── 08 Tree DFS
└── 20 Graph Islands (BFS/DFS)

📐 Intervals & Sorting
├── 04 Merge Intervals
└── 05 Cyclic Sort

⚙️ Heaps & Priority Queues
├── 09 Two Heaps
├── 13 Top K Elements
└── 14 K-way Merge

🔍 Search
└── 11 Modified Binary Search

🎨 Combinatorics
└── 10 Subsets

🧮 Dynamic Programming
├── 15 0/1 Knapsack
├── 17 Fibonacci DP
├── 18 Palindromic Subsequence
└── 19 Longest Common Substring

🕸️ Graphs (Advanced)
└── 16 Topological Sort
```

---

## 🚀 How to Use This Repo

```
Step 1 → Read the pattern explanation
Step 2 → Study the template code
Step 3 → Trace through the example manually
Step 4 → Solve 3 Easy problems to internalize
Step 5 → Move to Medium & Hard variants
Step 6 → Time yourself — aim for < 20 mins per Medium
```

---

## 🎯 Difficulty Color Legend

| Badge | Meaning |
|-------|---------|
| 🟢 Easy | Warm-up. Can be solved in < 10 mins after learning the pattern |
| 🟡 Medium | Most interview questions. 15–25 mins expected |
| 🔴 Hard | Senior roles / FAANG on-sites. 30–45 mins |

---

## 📅 Suggested Study Plan

| Week | Patterns |
|------|---------|
| Week 1 | Sliding Window, Two Pointers, Fast & Slow Pointers |
| Week 2 | Merge Intervals, Cyclic Sort, In-place Reversal |
| Week 3 | Tree BFS, Tree DFS, Graph Islands |
| Week 4 | Modified Binary Search, Bitwise XOR, Subsets |
| Week 5 | Top K Elements, Two Heaps, K-way Merge |
| Week 6 | Topological Sort, Monotonic Stack |
| Week 7–8 | DP Patterns: Knapsack, Fibonacci, Palindrome, LCS |
| Week 9+ | Mixed practice + revision |

---

## 🌟 Star History

If this helped you land an offer or crack an interview — **drop a ⭐ star** and share it with a friend who's grinding!

---

## 🤝 Contributing

Found a bug, want to add a problem, or have a better explanation?
Open a PR! All contributions are welcome.

---

<div align="center">

**Made with ❤️ for every developer grinding their way to their dream job.**

*"It always seems impossible until it's done." — Nelson Mandela*

</div>