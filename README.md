# Regular Expression Matching

This Java program implements regular expression matching between an input string `s` and a pattern `p`. The pattern supports two special characters:

- `.`: Matches any single character.
- `*`: Matches zero or more of the preceding element.

The matching must cover **the entire input string**.

## Examples

### Example 1
**Input:**  
s = `"aa"`  
p = `"a"`  
**Output:**  
`false`  
**Explanation:** Pattern "a" does not match the entire string "aa".

### Example 2
**Input:**  
s = `"aa"`  
p = `"a*"`  
**Output:**  
`true`  
**Explanation:** '*' allows repeating 'a' zero or more times. "a*" matches "aa".

### Example 3
**Input:**  
s = `"ab"`  
p = `".*"`  
**Output:**  
`true`  
**Explanation:** ".*" means zero or more of any character. So it matches "ab".

## Constraints

- 1 <= s.length <= 20
- 1 <= p.length <= 20
- `s` contains only lowercase English letters.
- `p` contains only lowercase English letters, `.` and `*`.
- Every `*` in `p` is guaranteed to have a valid preceding character.

## How It Works

The solution uses **dynamic programming** to build a table `dp[i][j]`, where each cell represents whether `s[0..i-1]` matches `p[0..j-1]`.

## Run the Code

Make sure you have Java installed.

To compile:
```bash
javac Solution.java
```

To run:
```bash
java Solution
```

You may add a `main` method for test inputs.
```

---

### 📝 Notes on `isMatch` Function Logic

```java
public boolean isMatch(String s, String p) {
    int m = s.length();
    int n = p.length();
    boolean[][] dp = new boolean[m + 1][n + 1];
    dp[0][0] = true; // Empty string matches empty pattern
```
- Initialize a 2D array `dp` where `dp[i][j]` is `true` if `s[0..i-1]` matches `p[0..j-1]`.

```java
    for (int j = 2; j <= n; j += 2) {
        if (p.charAt(j - 1) == '*' && dp[0][j - 2]) {
            dp[0][j] = true;
        }
    }
```
- Pre-fill `dp[0][j]` for patterns like `a*`, `a*b*`, etc. which can match an empty string.

```java
    for (int i = 1; i <= m; i++) {
        for (int j = 1; j <= n; j++) {
            char sc = s.charAt(i - 1);
            char pc = p.charAt(j - 1);
```
- Loop through all characters of `s` and `p`.

```java
            if (pc == '.' || pc == sc) {
                dp[i][j] = dp[i - 1][j - 1];
            }
```
- If current pattern character matches the string character, or is `.`, carry over the previous state.

```java
            else if (pc == '*') {
                char prev = p.charAt(j - 2);
                dp[i][j] = dp[i][j - 2]; // Zero occurrence

                if (prev == '.' || prev == sc) {
                    dp[i][j] = dp[i][j] || dp[i - 1][j]; // One or more occurrence
                }
            }
```
- If `*` appears, we check for:
  - Zero occurrences of the preceding character (`dp[i][j - 2]`).
  - One or more occurrences if previous char matches current `s`.

```java
    return dp[m][n];
}
```
- The final result is in `dp[m][n]`, which tells whether full `s` matches full `p`.

---

