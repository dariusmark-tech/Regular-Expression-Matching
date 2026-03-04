# Regular Expression Matching

> Java • Dynamic Programming • LeetCode Hard

A Java implementation of full regular expression matching using dynamic programming. The matching must cover the **entire** input string.

---

## Special Characters

| Character | Meaning |
|-----------|---------|
| `.` | Matches any single character |
| `*` | Matches zero or more of the preceding element |

---

## Examples

| # | s | p | Output | Explanation |
|---|---|---|--------|-------------|
| 1 | `"aa"` | `"a"` | `false` | `"a"` does not match the entire string `"aa"` |
| 2 | `"aa"` | `"a*"` | `true` | `*` repeats `'a'` zero or more times |
| 3 | `"ab"` | `".*"` | `true` | `".*"` matches any sequence of characters |

---

## Constraints

- `1 <= s.length <= 20`
- `1 <= p.length <= 20`
- `s` contains only lowercase English letters
- `p` contains only lowercase English letters, `.` and `*`
- Every `*` in `p` is guaranteed to have a valid preceding character

---

## How It Works

The solution builds a 2D boolean DP table `dp[i][j]`, where each cell represents whether `s[0..i-1]` matches `p[0..j-1]`.

### Step 1 — Initialization

- `dp[0][0] = true` — an empty string matches an empty pattern
- Pre-fill `dp[0][j]` for patterns like `a*`, `a*b*` that can match an empty string by treating `*` as zero occurrences

### Step 2 — Fill the Table

For each character pair (`sc` from `s`, `pc` from `p`), apply one of three rules:

| Condition | Action |
|-----------|--------|
| `pc == '.'` or `pc == sc` | Carry over `dp[i-1][j-1]` — characters match directly |
| `pc == '*'` (zero occurrences) | Use `dp[i][j-2]` — skip the `x*` pattern entirely |
| `pc == '*'` (one or more) | If prev char matches `sc`, also OR with `dp[i-1][j]` |

### Step 3 — Result

The answer is `dp[m][n]` — whether the full string `s` matches the full pattern `p`.

---

## How to Run

Make sure Java is installed.

**Compile:**
```bash
javac Solution.java
```

**Run:**
```bash
java Solution
```

> You may add a `main()` method to `Solution.java` to test with custom inputs.

---

## Code Walkthrough

### 1. Initialize DP Table

```java
boolean[][] dp = new boolean[m + 1][n + 1];
dp[0][0] = true; // Empty string matches empty pattern
```

### 2. Pre-fill for Patterns Matching Empty String

```java
for (int j = 2; j <= n; j += 2) {
    if (p.charAt(j - 1) == '*' && dp[0][j - 2]) {
        dp[0][j] = true;
    }
}
```

### 3. Fill DP Table

```java
for (int i = 1; i <= m; i++) {
    for (int j = 1; j <= n; j++) {
        char sc = s.charAt(i - 1);
        char pc = p.charAt(j - 1);

        if (pc == '.' || pc == sc) {
            dp[i][j] = dp[i - 1][j - 1];
        } else if (pc == '*') {
            char prev = p.charAt(j - 2);
            dp[i][j] = dp[i][j - 2]; // Zero occurrence
            if (prev == '.' || prev == sc) {
                dp[i][j] = dp[i][j] || dp[i - 1][j]; // One or more
            }
        }
    }
}
```

### 4. Return Result

```java
return dp[m][n];
```

---

## Complexity

| | |
|--|--|
| **Time** | `O(m × n)` — where `m = s.length`, `n = p.length` |
| **Space** | `O(m × n)` — for the DP table |
