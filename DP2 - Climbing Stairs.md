# Climbing Stairs — Dynamic Programming 

* **Recursion (Naive)**
* **Recursion + Memoization (Top-down DP)**
* **Tabulation (Bottom-up DP)**
* **Space Optimized DP**

All solutions are provided only in **C++**, along with explanation and complexity analysis.

---

## 📌 Problem Statement

You are climbing a staircase. It takes **n steps** to reach the top.

Each time you can either climb **1 step** or **2 steps**.

Return the **number of distinct ways** to reach the top.

This is identical to Fibonacci logic:

```
ways(n) = ways(n-1) + ways(n-2)
```

---

## 📘 Base Cases

```
ways(0) = 1   // one way: do nothing
ways(1) = 1   // one way: take a single step
```

---

# 1️⃣ Recursion (Naive)

This approach directly follows the recurrence but is **exponential**.

### C++ Code

```cpp
#include <iostream>
using namespace std;

int climb(int n) {
    if (n == 0 || n == 1) return 1;
    return climb(n - 1) + climb(n - 2);
}

int main() {
    int n = 5;
    cout << climb(n) << "\n";
}
```

### ❌ Drawbacks

* Recomputes many subproblems.
* Time complexity **O(2^n)** → unusable for large n.

---

# 2️⃣ Recursion + Memoization (Top-Down DP)

We store results in a DP array so each state is computed only once.

### C++ Code

```cpp
#include <iostream>
#include <vector>
using namespace std;

int solve(int n, vector<int> &dp) {
    if (n == 0 || n == 1) return 1;

    if (dp[n] != -1) return dp[n];

    return dp[n] = solve(n - 1, dp) + solve(n - 2, dp);
}

int climbStairs(int n) {
    vector<int> dp(n + 1, -1);
    return solve(n, dp);
}

int main() {
    int n = 5;
    cout << climbStairs(n) << "\n";
}
```

### ✅ Complexity

* **Time:** O(n)
* **Space:** O(n) for dp + recursion stack

---

# 3️⃣ Tabulation (Bottom-Up DP)

This approach builds the solution iteratively.

### C++ Code

```cpp
#include <iostream>
#include <vector>
using namespace std;

int climbStairs(int n) {
    if (n == 0 || n == 1) return 1;

    vector<int> dp(n + 1);
    dp[0] = 1;
    dp[1] = 1;

    for (int i = 2; i <= n; i++) {
        dp[i] = dp[i - 1] + dp[i - 2];
    }

    return dp[n];
}

int main() {
    int n = 5;
    cout << climbStairs(n) << "\n";
}
```

### ✅ Complexity

* **Time:** O(n)
* **Space:** O(n)

---

# 4️⃣ Space-Optimized DP (Iterative Fibonacci Style)

We only need last two states, so no array required.

### C++ Code

```cpp
#include <iostream>
using namespace std;

int climbStairs(int n) {
    if (n == 0 || n == 1) return 1;

    int prev2 = 1; // dp[0]
    int prev1 = 1; // dp[1]

    for (int i = 2; i <= n; i++) {
        int curr = prev1 + prev2;
        prev2 = prev1;
        prev1 = curr;
    }

    return prev1;
}

int main() {
    int n = 5;
    cout << climbStairs(n) << "\n";
}
```

### 🚀 Complexity

* **Time:** O(n)
* **Space:** O(1)

---

# 📊 Comparison Table

| Approach        | Time   | Space | Suitable For             |
| --------------- | ------ | ----- | ------------------------ |
| Recursion       | O(2^n) | O(n)  | Learning only            |
| Memoization     | O(n)   | O(n)  | Recursive DP lovers      |
| Tabulation      | O(n)   | O(n)  | Clean iterative solution |
| Space-Optimized | O(n)   | O(1)  | Best practical solution  |

---

# 🧪 Example

For `n = 5`:

```
ways = 8
Paths:
1-1-1-1-1
1-1-1-2
1-1-2-1
1-2-1-1
2-1-1-1
1-2-2
2-1-2
2-2-1
```

---

# 🎯 Final Notes

* Climbing Stairs is the **DP Hello World** problem.
* It teaches recurrence, memoization, state transition, and iterative DP.
* The best approach is **space-optimized DP**.

If you want, I can also add:

* Test cases
* A repository folder structure
* Comments for each line (beginner-friendly)
* LeetCode-style `class Solution` format
