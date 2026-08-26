# 🧮 Pattern 03: Prefix Sum

> [!NOTE]
> **Category:** Precomputation
> **Prerequisites:** `std::vector`, `std::unordered_map`
> **Difficulty:** Easy → Medium
> **Goal:** Reduce repeated range-sum queries from $O(N)$ each to $O(1)$ each, via a one-time $O(N)$ precomputation.

---

## 🎯 What Problem Does This Solve?

Given an array and a large number of queries asking for the sum of elements in a range `[l, r]`, the brute-force approach loops through the range every time: $O(N)$ per query.

The **Prefix Sum** technique precomputes cumulative sums once in $O(N)$, then answers every query in $O(1)$ via a single subtraction.

---

## 🧠 The Core Formula

Build a `prefix` array of size `n + 1` where `prefix[i]` = sum of `nums[0..i-1]`:

```
nums    = [3,  1,  4,  1,  5,  9,  2,  6]
prefix  = [0,  3,  4,  8,  9, 14, 23, 25, 31]
            ↑
         sentinel zero
```

**Universal Range Query (no edge cases):**
$$\text{sum}(l, r) = \text{prefix}[r+1] - \text{prefix}[l]$$

When `l == 0`: `prefix[r+1] - prefix[0]` = `prefix[r+1] - 0` ✅

---

## 💻 Standard Template

<details>
<summary><b>Reveal Template</b></summary>

```cpp
#include <vector>
using namespace std;

// Build
int n = nums.size();
vector<int> prefix(n + 1, 0); // sentinel at index 0
for (int i = 0; i < n; i++) {
    prefix[i + 1] = prefix[i] + nums[i];
}

// Query: sum of nums[l..r] (0-indexed, inclusive)
int rangeSum(int l, int r) {
    return prefix[r + 1] - prefix[l];
}
```

</details>

---

## 🔥 Advanced: Prefix Sum + HashMap

The most powerful application of Prefix Sum. Instead of querying a range, we **count subarrays** satisfying a condition.

**Key Identity:** `sum(l, r) = k` ⟺ `prefix[r+1] - prefix[l] = k` ⟺ `prefix[l] = prefix[r+1] - k`

So for each new prefix sum we compute, we ask: *"How many previous prefix sums equal `current - k`?"*
An `unordered_map<int, int>` answers this in $O(1)$.

> [!IMPORTANT]
> **Always initialize `map[0] = 1`** before the loop. This handles subarrays starting at index 0 (where `prefixSum == k`, and we need to find `prefix[0] = 0` in the map).

<details>
<summary><b>Reveal Template</b></summary>

```cpp
unordered_map<int, int> freq;
freq[0] = 1; // CRITICAL sentinel
int prefix_sum = 0, count = 0;

for (int x : nums) {
    prefix_sum += x;
    count += freq[prefix_sum - k]; // how many valid left boundaries exist?
    freq[prefix_sum]++;
}
```

</details>

---

## ⚠️ Common Traps

1. **Forgetting the sentinel:** Not initializing `prefix[0] = 0` (or `map[0] = 1`) will silently miss all subarrays that start at index 0.
2. **Using a direct prefix array (0-indexed):** Forces you to write `if (left == 0)` special-case logic. Always use the `n+1` sized array with the sentinel for clean, universal code.
3. **Integer overflow:** If `nums[i]` can be large and `n` is large, `prefix` values can exceed `INT_MAX`. Use `long long` if necessary.
