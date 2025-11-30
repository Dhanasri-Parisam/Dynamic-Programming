# 🐸 Frog Jump — Dynamic Programming (C++ Only)

This README explains the **Frog Jump** problem using:

* **Recursion (Naive)**
* **Recursion + Memoization (Top-down DP)**
* **Tabulation (Bottom-up DP)**
* **Space-Optimized DP**

## In the classic frog jump minimum-cost problem, greedy fails because taking the locally minimum-cost jump can block you from a cheaper total path that only becomes better after 1–2 future jumps.
## So greedy fails because choosing the smallest next jump (0→1→3) misses the future benefit of going through 60 (0→2→4), which gives a much lower total cost.

---

## 📌 Problem Statement

A frog is trying to reach the **N-th stone**.

The heights of the stones are given in an array `height[]`.

The frog can jump:

* **1 step** → cost = `abs(height[i] - height[i-1])`
* **2 steps** → cost = `abs(height[i] - height[i-2])`

Find the **minimum total energy** required to reach stone `n-1` from stone `0`.

---

## 🧠 Base Idea

Recursive relation:

```
dp[i] = min(
    dp[i-1] + abs(height[i]   - height[i-1]),
    dp[i-2] + abs(height[i]   - height[i-2])
)
```

Base case:

```
dp[0] = 0
```

---

# 1️⃣ Recursion (Naive)

Straight recursive solution without DP — very slow.

### C++ Code

```cpp
#include <bits/stdc++.h>
using namespace std;

int frog(int i, vector<int> &h) {
    if (i == 0) return 0;

    int one = frog(i - 1, h) + abs(h[i] - h[i - 1]);

    int two = INT_MAX;
    if (i > 1)
        two = frog(i - 2, h) + abs(h[i] - h[i - 2]);

    return min(one, two);
}

int main() {
    vector<int> h = {30, 10, 60, 10, 60, 50};
    cout << frog(h.size() - 1, h);
}
```

### ❌ Drawbacks

* Recomputes subproblems → **exponential time** O(2^n)
* Stack overflow for large n

---

# 2️⃣ Recursion + Memoization (Top-Down DP)

We store results of each index to avoid recalculation.

### C++ Code

```cpp
#include <bits/stdc++.h>
using namespace std;

int solve(int i, vector<int> &h, vector<int> &dp) {
    if (i == 0) return 0;

    if (dp[i] != -1) return dp[i];

    int one = solve(i - 1, h, dp) + abs(h[i] - h[i - 1]);

    int two = INT_MAX;
    if (i > 1)
        two = solve(i - 2, h, dp) + abs(h[i] - h[i - 2]);

    return dp[i] = min(one, two);
}

int frogJump(int n, vector<int> &h) {
    vector<int> dp(n, -1);
    return solve(n - 1, h, dp);
}

int main() {
    vector<int> h = {30, 10, 60, 10, 60, 50};
    cout << frogJump(h.size(), h);
}
```

### ✅ Complexity

* **Time:** O(n)
* **Space:** O(n) recursion + O(n) dp

---

# 3️⃣ Tabulation (Bottom-Up DP)

Iterative DP filling from index 0 to n-1.

### C++ Code

```cpp
#include <bits/stdc++.h>
using namespace std;

int frogJump(int n, vector<int> &h) {
    vector<int> dp(n);

    dp[0] = 0;

    for (int i = 1; i < n; i++) {
        int one = dp[i - 1] + abs(h[i] - h[i - 1]);

        int two = INT_MAX;
        if (i > 1)
            two = dp[i - 2] + abs(h[i] - h[i - 2]);

        dp[i] = min(one, two);
    }

    return dp[n - 1];
}

int main() {
    vector<int> h = {30, 10, 60, 10, 60, 50};
    cout << frogJump(h.size(), h);
}
```

### ✅ Complexity

* **Time:** O(n)
* **Space:** O(n)

---

# 4️⃣ Space-Optimized DP

We only need previous two states → store in variables.

### C++ Code

```cpp
#include <bits/stdc++.h>
using namespace std;

int frogJump(int n, vector<int> &h) {
    int prev = 0;
    int prev2 = 0;

    for (int i = 1; i < n; i++) {
        int one = prev + abs(h[i] - h[i - 1]);

        int two = INT_MAX;
        if (i > 1)
            two = prev2 + abs(h[i] - h[i - 2]);

        int curr = min(one, two);
        prev2 = prev;
        prev = curr;
    }

    return prev;
}

int main() {
    vector<int> h = {30, 10, 60, 10, 60, 50};
    cout << frogJump(h.size(), h);
}
```

### 🚀 Complexity

* **Time:** O(n)
* **Space:** **O(1)** (best solution)

---

# 📊 Comparison Table

| Approach        | Time   | Space | Notes                    |
| --------------- | ------ | ----- | ------------------------ |
| Recursion       | O(2^n) | O(n)  | Too slow                 |
| Memoization     | O(n)   | O(n)  | Best for recursive logic |
| Tabulation      | O(n)   | O(n)  | Clean & simple DP        |
| Space-Optimized | O(n)   | O(1)  | Best practical method    |

---

# 🧪 Example

Input:

```
height = [30, 10, 60, 10, 60, 50]
```

Output:

```
40
```

Explanation:
The optimal path minimizes the total energy using 1-step or 2-step jumps.

---

# 🎯 Final Notes

* Frog Jump is a foundational DP problem.
* Learning it helps to understand DP transitions clearly.
* Always try space optimization for best performance.

If you'd like, I can also add:

* Frog Jump (K steps version)
* LeetCode / GFG class-based template
* Diagram of jumps
* Folder structure for GitHub repo
