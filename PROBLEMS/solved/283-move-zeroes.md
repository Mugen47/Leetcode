# 📝 283 - Move Zeroes

> [!NOTE]
> Link: [LeetCode 283](https://leetcode.com/problems/move-zeroes/)
> Difficulty: Easy
> Pattern: Two Pointers (Same Direction / Read & Write)

### 1. Constraints
*   `1 <= nums.length <= 10^4`
*   `-2^31 <= nums[i] <= 2^31 - 1`
*   You must do this **in-place** without making a copy of the array.
*   Minimize the total number of operations.
*   Expected Time Complexity: $O(N)$

### 2. First Thoughts
We need to move elements matching a specific condition (being zero) to the end of the array, while preserving the relative order of the other elements. This is a classic filtering problem. Since we can't allocate a new array, we must use Read/Write pointers (Same Direction Two Pointers). 

### 3. What Makes This Problem Difficult?
It is tempting to write complex logic to "find the next zero" and "find the next non-zero" and swap them. This leads to nested loops and ugly boundary conditions. The breakthrough is realizing we don't care about the zeros at all—we only care about moving the non-zeroes to the front.

---

<details>
<summary><b>🧠 Pattern Recognition & Hints</b></summary>

### Pattern Recognition
"In-place", "Maintain relative order", "Filtering elements". This is the textbook definition of the Read/Write (Same Direction) Two Pointers pattern.

### Hint 1
Instead of focusing on where the zeros go, focus on where the non-zeroes go.

### Hint 2
If you have a `slow` pointer keeping track of the next available slot for a non-zero number, what should you do when your `fast` pointer finds a non-zero number?

</details>

---

<details>
<summary><b>💡 Core Insight</b></summary>
The `fast` pointer's only job is to scan for valid data (non-zeroes). The `slow` pointer's only job is to point to the next available position to store valid data. When `fast` finds valid data, we `swap` it with the element at `slow`. Because `fast` is always ahead of or equal to `slow`, anything at `slow` is either a processed element (if `slow == fast`) or a zero that `fast` has already passed!
</details>

---

<details open>
<summary><b>💻 Implementation</b></summary>

```cpp
class Solution {
public:
    void moveZeroes(vector<int>& nums) {
        int slow = 0;
        for (int fast = 0; fast < nums.size(); fast++) {
            if (nums[fast] != 0) {
                // If fast == slow, it swaps with itself (harmless).
                // If fast > slow, nums[slow] is guaranteed to be a 0 that fast already passed.
                swap(nums[slow], nums[fast]);
                slow++;
            }
        }
    }
};
```

</details>

---

<details>
<summary><b>🔍 Analysis & Edge Cases</b></summary>

### Correctness Proof
The `slow` pointer guarantees that all elements before it are non-zero and in their original order. When `fast` finds a non-zero, it swaps it into the `slow` position. Any element at `slow` must be a zero because `fast` already evaluated it and didn't increment `slow`. 

### Complexity
*   **Time:** $O(N)$ - A single pass through the array.
*   **Space:** $O(1)$ - Only using two integer pointers.

### Edge Cases
*   **No zeroes:** `[1, 2, 3]`. `fast` and `slow` move together, doing `swap(nums[0], nums[0])`, which takes negligible time and works perfectly without adding `if` checks.
*   **All zeroes:** `[0, 0, 0]`. `fast` goes to the end, `slow` stays at 0. No swaps occur. Array remains unchanged. Correct.
*   **Zeroes at the end:** `[1, 2, 0, 0]`. `fast` and `slow` move together for 1 and 2. Then `fast` skips the zeroes. Correct.

</details>

---

### 🧠 What I Should Remember
Never over-complicate Read/Write pointers with `flags` or nested `while` loops. The logic is always: *If the reader finds valid data, write it to the slow pointer, and advance the slow pointer.* C++ `std::swap(x, x)` is completely safe and avoids unnecessary branching.
