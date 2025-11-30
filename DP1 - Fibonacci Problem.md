# Fibonacci Series — Dynamic Programming

A concise, practical README that explains the Fibonacci series and demonstrates **three primary approaches** to compute it: **recursion (naive)**, **memoization (top-down DP)**, and **tabulation (bottom-up DP)** — with full code examples, step-by-step explanations, complexity analysis, and tips for choosing the right method.

---
## Introduction

The Fibonacci sequence is a classic example to learn recursion and dynamic programming. It is defined as:

```
F(0) = 0
F(1) = 1
F(n) = F(n-1) + F(n-2)  for n >= 2
```

The sequence begins: `0, 1, 1, 2, 3, 5, 8, 13, ...`.

This README shows three common ways to compute `F(n)` and explains why dynamic programming dramatically improves performance compared to naive recursion.

---

## Problem Statement

Given an integer `n >= 0`, compute the `n`-th Fibonacci number `F(n)`.

We will show implementations in three languages (Python, Java, C++) for each approach so you can pick the language you prefer.

---

## Fibonacci by Recursion (Naive)

### Code — Python

```python
# naive_recursive.py

def fib_recursive(n):
    if n < 0:
        raise ValueError("n must be non-negative")
    if n == 0:
        return 0
    if n == 1:
        return 1
    return fib_recursive(n - 1) + fib_recursive(n - 2)

if __name__ == "__main__":
    print(fib_recursive(10))  # 55
```

### Code — Java

```java
// NaiveRecursive.java
public class NaiveRecursive {
    public static long fib(int n) {
        if (n < 0) throw new IllegalArgumentException("n must be >= 0");
        if (n == 0) return 0;
        if (n == 1) return 1;
        return fib(n - 1) + fib(n - 2);
    }

    public static void main(String[] args) {
        System.out.println(fib(10)); // 55
    }
}
```

### Code — C++

```cpp
// naive_recursive.cpp
#include <iostream>
using namespace std;

long long fib(long long n) {
    if (n < 0) throw invalid_argument("n must be >= 0");
    if (n == 0) return 0;
    if (n == 1) return 1;
    return fib(n - 1) + fib(n - 2);
}

int main() {
    cout << fib(10) << "\n"; // 55
    return 0;
}
```

### How it works

This directly follows the Fibonacci definition and calls `fib` recursively for smaller values.

### Complexity

* Time complexity: **O(2^n)** (exponential). Many calls are repeated.
* Space complexity: **O(n)** due to recursion call stack.

### When to use / drawbacks

* Easy to understand and implement, good for teaching recursion.
* Not suitable for `n > ~40` in practice due to exponential time.

---

## Fibonacci with Memoization (Top-down DP)

Memoization stores results of expensive function calls and returns the cached result when the same inputs occur again.

### Code — Python

```python
# memoization.py

def fib_memo(n, memo=None):
    if n < 0:
        raise ValueError("n must be non-negative")
    if memo is None:
        memo = {0: 0, 1: 1}
    if n in memo:
        return memo[n]
    memo[n] = fib_memo(n - 1, memo) + fib_memo(n - 2, memo)
    return memo[n]

if __name__ == "__main__":
    print(fib_memo(50))  # 12586269025
```

### Code — Java

```java
// Memoization.java
import java.util.HashMap;
import java.util.Map;

public class Memoization {
    private static Map<Integer, Long> memo = new HashMap<>();

    public static long fib(int n) {
        if (n < 0) throw new IllegalArgumentException("n must be >= 0");
        memo.putIfAbsent(0, 0L);
        memo.putIfAbsent(1, 1L);
        if (memo.containsKey(n)) return memo.get(n);
        long value = fib(n - 1) + fib(n - 2);
        memo.put(n, value);
        return value;
    }

    public static void main(String[] args) {
        System.out.println(fib(50));
    }
}
```

### Code — C++

```cpp
// memoization.cpp
#include <iostream>
#include <vector>

using namespace std;

long long fib_memo(int n, vector<long long>& memo) {
    if (n == 0) return 0;
    if (n == 1) return 1;
    if (memo[n] != -1) return memo[n];
    memo[n] = fib_memo(n - 1, memo) + fib_memo(n - 2, memo);
    return memo[n];
}

int main() {
    int n = 50;
    vector<long long> memo(n + 1, -1);
    cout << fib_memo(n, memo) << "\n";
}
```

### How it works

* Start with the recursive approach but cache each computed `F(k)`.
* When `fib(k)` is needed again, return the cached value instead of recomputing.

### Complexity

* Time complexity: **O(n)** — each state `0..n` is computed once.
* Space complexity: **O(n)** for the memo table plus recursion stack **O(n)**.

### Implementation tips

* Use dictionaries/maps (Python `dict`, Java `HashMap`, or C++ `vector`) to store computed values.
* Initialize base cases in the memo to avoid extra checks.

---

## Fibonacci with Tabulation (Bottom-up DP)

Tabulation fills a table iteratively from the base up to `n`. It avoids recursion entirely.

### Code — Python

```python
# tabulation.py

def fib_tabulation(n):
    if n < 0:
        raise ValueError("n must be non-negative")
    if n == 0: return 0
    dp = [0] * (n + 1)
    dp[0], dp[1] = 0, 1
    for i in range(2, n + 1):
        dp[i] = dp[i - 1] + dp[i - 2]
    return dp[n]

if __name__ == "__main__":
    print(fib_tabulation(50))
```

### Code — Java

```java
// Tabulation.java
public class Tabulation {
    public static long fib(int n) {
        if (n < 0) throw new IllegalArgumentException("n must be >= 0");
        if (n == 0) return 0;
        long[] dp = new long[n + 1];
        dp[0] = 0;
        dp[1] = 1;
        for (int i = 2; i <= n; ++i) dp[i] = dp[i - 1] + dp[i - 2];
        return dp[n];
    }

    public static void main(String[] args) {
        System.out.println(fib(50));
    }
}
```

### Code — C++

```cpp
// tabulation.cpp
#include <iostream>
#include <vector>

using namespace std;

long long fib_tab(int n) {
    if (n == 0) return 0;
    vector<long long> dp(n + 1);
    dp[0] = 0;
    dp[1] = 1;
    for (int i = 2; i <= n; ++i)
        dp[i] = dp[i - 1] + dp[i - 2];
    return dp[n];
}

int main() {
    cout << fib_tab(50) << "\n";
}
```

### How it works

* Build an array `dp` such that `dp[i] = F(i)`.
* Compute `dp[2], dp[3], ..., dp[n]` sequentially using the relation.

### Complexity

* Time complexity: **O(n)**
* Space complexity: **O(n)** for the `dp` array

### Space-Optimized Variant

You only need the last two values to compute the next Fibonacci number. This reduces the space to **O(1)**.

#### Code — Python (space optimized)

```python
# fib_space_optimized.py

def fib_space(n):
    if n < 0: raise ValueError("n must be non-negative")
    if n == 0: return 0
    a, b = 0, 1
    for _ in range(2, n + 1):
        a, b = b, a + b
    return b if n > 0 else a

if __name__ == "__main__":
    print(fib_space(50))
```

#### Complexity (space-optimized)

* Time: **O(n)**
* Space: **O(1)** (only two variables)

---

## Comparison & Choosing the Right Approach

* **Recursion (naive)**: simplest but exponential time. Use only for learning or tiny `n`.
* **Memoization (top-down DP)**: easy to convert from recursion; good when you want to keep recursion style. Time and space **O(n)**.
* **Tabulation (bottom-up DP)**: iterative, avoids recursion overhead. Time **O(n)** and space can be **O(n)** or **O(1)** if optimized.

For production code, prefer tabulation with space optimization (or memoization for clarity if recursion is preferred).

---

## Test Cases & Examples

|  n | Expected F(n) |
| -: | ------------: |
|  0 |             0 |
|  1 |             1 |
|  2 |             1 |
|  5 |             5 |
| 10 |            55 |
| 20 |          6765 |
| 50 |   12586269025 |

**Edge cases**: negative `n` (invalid), very large `n` (use 64-bit integers or big integers / `BigInteger` in Java / Python `int` handles big integers), or performance concerns for very large `n` (consider matrix exponentiation or fast doubling algorithms which run in O(log n)).
