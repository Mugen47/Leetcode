# 🔥 01: Two Pointers

## 🎯 What Problem Does This Pattern Solve?

The Two Pointers pattern allows us to process an array or a list from two different positions simultaneously. 
Instead of a nested loop (which takes $O(N^2)$ time), we track two indices (pointers) and move them strategically based on the problem's constraints, reducing the time complexity to $O(N)$.

---

## 🧠 Intuition

Imagine you have a sorted list of numbers and want to find two numbers that sum to a target. 
If you start one pointer at the smallest number (left) and one at the largest (right), you can evaluate their sum. 
*   If the sum is **too small**, the only way to increase it is to move the left pointer forward.
*   If the sum is **too large**, the only way to decrease it is to move the right pointer backward.

By exploiting the sorted property, you systematically eliminate impossible pairs without checking them all.

---

## 🔍 Recognition Signals

> [!TIP]
> **How to know you should use Two Pointers?**
> Watch for these clues in the problem description:

1.  **"Sorted array"** or **"Sorted sequence"** (Especially when searching for pairs/triplets).
2.  **"Find a pair/triplet"** that satisfies a condition (e.g., sum, difference).
3.  **"In-place manipulation"** (e.g., reversing an array, moving zeros to the end, removing duplicates).
4.  **"Comparing elements from the ends"** (e.g., Palindromes).

---

## 🧩 Core Invariants

> [!IMPORTANT]
> The defining invariant of the *Opposite Direction Two Pointers* is:
> **Every time a pointer moves, we are safely eliminating an entire row/column of the theoretical $O(N^2)$ search space because we mathematically proved they cannot be the answer.**

---

## 📐 Visual Example (Opposite Direction)

```text
Target Sum = 8

[1, 2, 3, 4, 6]
 ↑           ↑
 L           R     (1 + 6 = 7) → Too small, move L right

[1, 2, 3, 4, 6]
    ↑        ↑
    L        R     (2 + 6 = 8) → Target found!
```

---

## 🪜 The Sub-Patterns

The Two Pointers technique actually breaks down into three distinct sub-patterns:

<details>
<summary><b>1️⃣ Opposite Direction (Collision)</b></summary>

*   **Setup:** `left = 0`, `right = n - 1`
*   **Action:** Pointers move towards each other until they meet (`while left < right`).
*   **Common Use:** Two Sum II, Valid Palindrome, Container With Most Water.
</details>

<details>
<summary><b>2️⃣ Same Direction (Fast & Slow / Read & Write)</b></summary>

*   **Setup:** Both pointers start at index `0`. 
*   **Action:** One pointer iterates through the array (Read/Fast), while the other tracks the position for valid data (Write/Slow).
*   **Common Use:** Remove Duplicates from Sorted Array, Move Zeroes.
</details>

<details>
<summary><b>3️⃣ Two Arrays (Merging)</b></summary>

*   **Setup:** `p1 = 0` (for Array A), `p2 = 0` (for Array B).
*   **Action:** Advance the pointer that satisfies the sorting or merging condition.
*   **Common Use:** Merge Sorted Arrays, Intersection of Two Arrays.
</details>

---

## 🚨 Common Mistakes

> [!WARNING]
> **Trap 1:** Forgetting that *Opposite Direction* Two Pointers on arrays involving sums/differences **requires the array to be sorted**. If it's unsorted, sorting it takes $O(N \log N)$. 
> 
> **Trap 2:** Off-by-one errors. Should the loop be `while left < right` or `while left <= right`? (For pairs, it's strictly `<`. For binary search or finding a single element, it's `<=`).

---

## ⚖️ Similar Patterns

| This Pattern | That Pattern | The Difference |
| :--- | :--- | :--- |
| **Two Pointers** | **Sliding Window** | Two pointers evaluate elements *independently* (e.g. `A[left] + A[right]`). Sliding window evaluates the *continuous range* between them (e.g. `sum(A[left...right])`). |
| **Two Pointers** | **Binary Search** | Binary search cuts the search space in half. Two pointers usually shrinks it by one element at a time. |

---

## 💻 Template (Opposite Direction)

<details>
<summary><b>Reveal Template</b></summary>

```cpp
#include <vector>

using namespace std;

vector<int> twoPointersOpposite(const vector<int>& arr, int target) {
    int left = 0;
    int right = arr.size() - 1;
    
    while (left < right) {
        // Evaluate condition
        int current_val = arr[left] + arr[right]; // Example condition
        
        if (current_val == target) {
            return {left, right};
        } else if (current_val < target) {
            left++;  // Need a larger sum
        } else {
            right--; // Need a smaller sum
        }
    }
    
    return {-1, -1};
}
```
</details>

---

## 🧠 Think Before Looking (Active Recall)

Before moving to the problems, consider this scenario:
**You are given an array of integers, and you need to square every number and return a new array sorted in non-decreasing order. The input array is sorted, but it contains negative numbers.**

<details>
<summary>💡 Hint 1</summary>
Squaring a negative number makes it positive. The largest squares will be at the extreme left (large negatives) or extreme right (large positives).
</details>

<details>
<summary>💡 Hint 2</summary>
Where is the smallest square? It's somewhere in the middle, near zero. It's hard to find the smallest first.
</details>

<details>
<summary>🧠 Core Insight</summary>
Since the largest squares are at the ends, we can use two pointers at `0` and `n-1`. We compare the absolute values (or squares) of the elements at the pointers. The larger one gets placed at the end of our result array, and we move that pointer inward. We build the result array backwards!
</details>

---

## 📚 Problem Index

| LeetCode # | Title | Sub-Pattern | Status |
| :---: | :--- | :--- | :---: |
| *(TBD)* | *(Ready for First Problem)* | - | ⚪ |

---

# ⚡ Quick Reference
*   **Recognition:** Sorted array, searching for pairs, in-place removal, palindromes.
*   **Core Idea:** Exploit ordering to eliminate $O(N^2)$ brute force paths, yielding $O(N)$ time.
*   **Complexity:** Time $O(N)$, Space $O(1)$ (usually).
*   **When NOT to use:** When you need a contiguous subarray sum (use Sliding Window / Prefix Sum) or when finding subsets (use Backtracking).
