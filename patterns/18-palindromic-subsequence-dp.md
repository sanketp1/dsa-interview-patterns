<div align="center">

# 🪞 Pattern 18 — Palindromic Subsequence (DP)

[![Difficulty](https://img.shields.io/badge/Difficulty-Medium-yellow?style=flat-square)](#)
[![Problems](https://img.shields.io/badge/Problems-8+-blue?style=flat-square)](#practice-problems)

> **Core Idea:** dp[i][j] = result for substring s[i..j]. Build from small subproblems outward.

</div>

---

## 🧠 What is Palindromic Subsequence DP?

Problems involving **palindromes and substrings** use interval DP: dp[i][j] answers for range [i, j]. Expand ranges iteratively.

---

## 🔑 When to Use It?

```
✅ Longest palindromic subsequence
✅ Longest palindromic substring
✅ Min cuts for palindromes
✅ Count palindromic substrings
✅ Edit distance (transform to palindrome)
✅ Distinct palindromic subsequences
```

---

## 📐 DP Expansion Visualization

```
String: "bbbab"

    b  b  b  a  b
b   T  T  T  F  F
b      T  T  F  T
b         T  F  F
a            T  F
b               T

Diagonal: single chars (palindrome)
Expand: check dp[i+1][j-1] + match s[i]==s[j]
```

---

## 📋 Template Code

### Longest Palindromic Substring
```java
public String longestPalindrome(String s) {
    int n = s.length();
    boolean[][] dp = new boolean[n][n];
    int start = 0, maxLen = 1;
    
    // Every single char is palindrome
    for (int i = 0; i < n; i++) {
        dp[i][i] = true;
    }
    
    // Check length 2
    for (int i = 0; i < n - 1; i++) {
        if (s.charAt(i) == s.charAt(i + 1)) {
            dp[i][i + 1] = true;
            start = i;
            maxLen = 2;
        }
    }
    
    // Check length 3 and more
    for (int len = 3; len <= n; len++) {
        for (int i = 0; i <= n - len; i++) {
            int j = i + len - 1;
            
            if (s.charAt(i) == s.charAt(j) && dp[i + 1][j - 1]) {
                dp[i][j] = true;
                start = i;
                maxLen = len;
            }
        }
    }
    
    return s.substring(start, start + maxLen);
}
```

### Minimum Cut for Palindromes
```java
public int minCut(String s) {
    int n = s.length();
    boolean[][] isPalin = new boolean[n][n];
    int[] dp = new int[n];  // min cuts needed
    
    // Build palindrome table
    for (int i = 0; i < n; i++) {
        dp[i] = i;  // worst case: i cuts (split every char)
    }
    
    for (int i = 0; i < n; i++) {
        for (int j = 0; j <= i; j++) {
            if (s.charAt(i) == s.charAt(j) && 
                (i - j <= 1 || isPalin[j + 1][i - 1])) {
                isPalin[j][i] = true;
                dp[i] = Math.min(dp[i], (j > 0 ? dp[j - 1] + 1 : 0));
            }
        }
    }
    
    return dp[n - 1];
}
```

---

## 🔬 Detailed Example — Longest Palindromic Subsequence

**Problem:** Find longest subsequence that is palindrome.

**Input:** `"bbbab"` → **Output:** `"bbbb"` (length 4)

```java
public int longestPalindromeSubseq(String s) {
    int n = s.length();
    int[][] dp = new int[n][n];
    
    // Every single char has length 1
    for (int i = 0; i < n; i++) {
        dp[i][i] = 1;
    }
    
    // Check substrings of increasing length
    for (int len = 2; len <= n; len++) {
        for (int i = 0; i <= n - len; i++) {
            int j = i + len - 1;
            
            if (s.charAt(i) == s.charAt(j)) {
                dp[i][j] = dp[i + 1][j - 1] + 2;
            } else {
                dp[i][j] = Math.max(dp[i + 1][j], dp[i][j - 1]);
            }
        }
    }
    
    return dp[0][n - 1];
}

// Trace for "bbbab":
// dp[0][0]=1, dp[1][1]=1, ..., dp[4][4]=1
// len=2: b,b match → dp[0][1]=2; b,b match → dp[1][2]=2; ...
// len=3: b,b match → dp[0][2]=2+1=3 (b + dp[1][1] + b)
// len=4: b,a no match → dp[0][3]=max(dp[1][3], dp[0][2])=3
// len=5: b,b match → dp[0][4]=3+2=5 (wait, should be 4)
// Recalc: b b b a b → "bbbb" (indices 0,1,2,4)
```

---

## 💡 Key Insight

> **Interval DP builds outward.** dp[i][j] = solution for s[i..j]. If ends match, add 2 to dp[i+1][j-1]. Else, max of skipping either end.

---

## 🧩 Common Variations

| Problem | DP Definition |
|---------|--------------|
| Longest pal substr | Boolean if palindrome |
| Longest pal subseq | Length of longest |
| Min cuts | Min cuts needed |
| Count distinct | Count all |

---

## 🏋️ Practice Problems

### 🟡 Medium
| # | Problem | Platform | Key Trick |
|---|---------|----------|-----------|
| 1 | [Longest Palindromic Substring](https://leetcode.com/problems/longest-palindromic-substring/) | LeetCode #5 | Expand outward |
| 2 | [Palindrome Partitioning II](https://leetcode.com/problems/palindrome-partitioning-ii/) | LeetCode #132 | Min cuts |
| 3 | [Longest Palindromic Subsequence](https://leetcode.com/problems/longest-palindromic-subsequence/) | LeetCode #516 | Interval DP |
| 4 | [Count Different Palindromic Subsequences](https://leetcode.com/problems/count-different-palindromic-subsequences/) | LeetCode #730 | Count distinct |

---

## 📊 Complexity

| Operation | Time | Space |
|-----------|------|-------|
| Longest substr | O(n²) | O(n²) |
| Min cuts | O(n²) | O(n²) |
| Subsequence | O(n²) | O(n²) |

---

## 🔗 Navigation

[← Fibonacci DP](./17-fibonacci-dp.md) | [Back to Main](../README.md) | [Next: Longest Common Substring →](./19-longest-common-substring-dp.md)
