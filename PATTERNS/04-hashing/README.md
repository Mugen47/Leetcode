# 🗂️ Pattern 04: Hashing

> [!NOTE]
> **Category:** Fast Lookups
> **Prerequisites:** `std::unordered_map`, `std::unordered_set`
> **Difficulty:** Easy → Medium
> **Goal:** Replace $O(N^2)$ brute-force pair/membership searching with $O(N)$ hash lookups.

---

## 🎯 What Problem Does This Solve?

Any problem where you need to ask: *"Have I seen this value before?"* or *"Does the complement / partner of this element exist?"*

Without hashing: loop through the entire array to search → $O(N)$ per lookup → $O(N^2)$ total.
With hashing: store values as keys → $O(1)$ lookup → $O(N)$ total.

---

## 🧩 The Two Archetypes

### 1. `unordered_set` — Membership Check
Use when you only need to know **if** a value exists.
```
"Does element X exist in the collection?"
```

### 2. `unordered_map<K, V>` — Value + Metadata Lookup
Use when you need to know **if** a value exists AND retrieve metadata about it (index, frequency, etc.).
```
"Does element X exist, and if so, at which index / how many times?"
```

---

## 💻 Standard Templates

<details>
<summary><b>Template 1: Complement Search (Two Sum style)</b></summary>

```cpp
// For each element, check if its "partner" already exists in the map.
// CRITICAL: Check BEFORE inserting. This guarantees distinct indices.
unordered_map<int, int> seen; // value -> index

for (int i = 0; i < nums.size(); i++) {
    int complement = target - nums[i];
    if (seen.count(complement)) {
        return {seen[complement], i}; // found pair
    }
    seen[nums[i]] = i; // insert AFTER checking
}
```

</details>

<details>
<summary><b>Template 2: Frequency Counting</b></summary>

```cpp
// Count occurrences of each element.
unordered_map<int, int> freq;
for (int x : nums) freq[x]++;

// Then query:
if (freq[target] > 1) { /* duplicate exists */ }
```

</details>

<details>
<summary><b>Template 3: Prefix Sum + Frequency (Subarray Counting)</b></summary>

```cpp
// Count subarrays satisfying a condition.
unordered_map<int, int> prefix_freq;
prefix_freq[0] = 1; // CRITICAL sentinel
int prefix = 0, count = 0;

for (int x : nums) {
    prefix += x;
    count += prefix_freq[prefix - k];
    prefix_freq[prefix]++;
}
```

</details>

---

## ⚠️ Common Traps

1. **Checking AFTER inserting:** In complement-search problems, inserting first and then checking allows `nums[i]` to match with itself (wrong). Always check before inserting.
2. **Forgetting `map[0] = 1` sentinel:** In prefix-sum + frequency problems, forgetting this silently misses all subarrays starting at index 0.
3. **Using `map[key]` instead of `map.count(key)` for existence checks:** Accessing a non-existent key via `map[key]` silently inserts a default value (0) into the map, polluting it. Use `map.count(key)` or `map.find(key) != map.end()` instead.
