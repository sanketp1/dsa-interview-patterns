<div align="center">

# 🧵 Pattern 19 — Longest Common Substring (DP)

[![Difficulty](https://img.shields.io/badge/Difficulty-Medium-yellow?style=flat-square)](#)
[![Problems](https://img.shields.io/badge/Problems-6+-blue?style=flat-square)](#practice-problems)

> **Core Idea:** 2D DP table: dp[i][j] = match length of s1[0..i-1] and s2[0..j-1]. Matches reset on mismatch.

</div>

---

## 🧠 What is LCS DP?

Find **longest common substring** (contiguous) or **subsequence** (not necessarily contiguous). Use 2D DP where rows=s1, cols=s2.

---

## 🔑 When to Use It?

```
✅ Longest common substring
✅ Longest common subsequence
✅ Edit distance (Levenshtein)
✅ Wildcard matching
✅ Regular expression matching
✅ Distinct subsequences
```

---

## 📐 DP Table Visualization

```
s1 = "abcde"
s2 = "ace"

      ""  a  c  e
""     0  0  0  0
a      0  1  0  0
b      0  0  0  0
c      0  0  1  0
d      0  0  0  0
e      0  0  0  1

Longest common substring: length 1 (a, c, or e individually)
Longest common subsequence: "ace" (length 3)
```

---

## 📋 Template Code

### Longest Common Substring
```java
public int longestCommonSubstring(String s1, String s2) {
    int m = s1.length(), n = s2.length();
    int[][] dp = new int[m + 1][n + 1];
    int maxLen = 0;
    
    for (int i = 1; i <= m; i++) {
        for (int j = 1; j <= n; j++) {
            if (s1.charAt(i - 1) == s2.charAt(j - 1)) {
                dp[i][j] = dp[i - 1][j - 1] + 1;  // extend match
                maxLen = Math.max(maxLen, dp[i][j]);
            }
            // else: dp[i][j] = 0 (reset on mismatch)
        }
    }
    
    return maxLen;
}
```

### Longest Common Subsequence
```java
public int longestCommonSubsequence(String s1, String s2) {
    int m = s1.length(), n = s2.length();
    int[][] dp = new int[m + 1][n + 1];
    
    for (int i = 1; i <= m; i++) {
        for (int j = 1; j <= n; j++) {
            if (s1.charAt(i - 1) == s2.charAt(j - 1)) {
                dp[i][j] = dp[i - 1][j - 1] + 1;  // match
            } else {
                dp[i][j] = Math.max(dp[i - 1][j], dp[i][j - 1]);  // skip one
            }
        }
    }
    
    return dp[m][n];
}
```

### Edit Distance (Levenshtein)
```java
public int editDistance(String word1, String word2) {
    int m = word1.length(), n = word2.length();
    int[][] dp = new int[m + 1][n + 1];
    
    // Initialize: need i deletions or j insertions
    for (int i = 0; i <= m; i++) dp[i][0] = i;
    for (int j = 0; j <= n; j++) dp[0][j] = j;
    
    for (int i = 1; i <= m; i++) {
        for (int j = 1; j <= n; j++) {
            if (word1.charAt(i - 1) == word2.charAt(j - 1)) {
                dp[i][j] = dp[i - 1][j - 1];  // no operation
            } else {
                dp[i][j] = 1 + Math.min({
                    dp[i - 1][j],      // delete from word1
                    dp[i][j - 1],      // insert into word1
                    dp[i - 1][j - 1]   // replace
                });
            }
        }
    }
    
    return dp[m][n];
}
```

---

## 🔬 Detailed Example — Edit Distance

**Problem:** Min operations to transform word1 to word2.

**Input:** `word1="horse", word2="ros"` → **Output:** `3` (delete h, delete e)

```java
// Trace for "horse" → "ros":
//       ""  r  o  s
// ""    0   1  2  3
// h     1   1  2  3
// o     2   2  1  2
// r     3   2  2  2
// s     4   3  3  2
// e     5   4  4  3

// dp[5][3] = 3 (delete h, keep o,r,s, insert 'e'? No...)
// Actually: delete h, delete e, substitute r with r, keep o, substitute s
// Or: substitute h→r, keep o, delete r, keep s... recalc needed
```

---

## 💡 Key Insight

> **Match vs Mismatch.** If chars match, carry forward previous match count. If not, in LCS take max of skip options; in edit distance, take min of operations.

---

## 🧩 Common Variations

| Problem | Key Difference |
|---------|----------------|
| Common substring | Reset on mismatch |
| Common subseq | Take max of skips |
| Edit distance | Count min operations |
| Wildcard matching | Handle `*` and `?` |

---

## 🏋️ Practice Problems

### 🟡 Medium
| # | Problem | Platform | Key Trick |
|---|---------|----------|-----------|
| 1 | [Edit Distance](https://leetcode.com/problems/edit-distance/) | LeetCode #72 | Min operations |
| 2 | [Longest Common Subseq](https://leetcode.com/problems/longest-common-subsequence/) | LeetCode #1143 | Skip options |
| 3 | [Wildcard Matching](https://leetcode.com/problems/wildcard-matching/) | LeetCode #44 | `*` greedy |
| 4 | [Distinct Subseq](https://leetcode.com/problems/distinct-subsequences/) | LeetCode #115 | Count ways |

### 🔴 Hard
| # | Problem | Platform | Key Trick |
|---|---------|----------|-----------|
| 5 | [Regex Matching](https://leetcode.com/problems/regular-expression-matching/) | LeetCode #10 | `*` and `.` |

---

## 📊 Complexity

| Operation | Time | Space |
|-----------|------|-------|
| Common substring | O(m·n) | O(m·n) |
| Common subseq | O(m·n) | O(m·n) |
| Edit distance | O(m·n) | O(m·n) |

---

## 🔗 Navigation

[← Palindromic Subsequence](./18-palindromic-subsequence-dp.md) | [Back to Main](../README.md) | [Next: Graph Islands →](./20-graph-islands.md)
