# 📝 209 - Minimum Size Subarray Sum

> [!NOTE]
> Link: [LeetCode 209](https://leetcode.com/problems/minimum-size-subarray-sum/)
> Difficulty: Medium
> Pattern: Sliding Window (Dynamic / Caterpillar)

### 1. Constraints
*   `1 <= target <= 10^9`
*   `1 <= nums.length <= 10^5`
*   `1 <= nums[i] <= 10^4`
*   Expected Time Complexity: $O(N)$

### 2. First Thoughts
We are looking for a contiguous subarray, which screams Sliding Window. However, we do not know the size of the window in advance. It must expand to reach the target, and then shrink to find the *minimal* length. This requires a dynamic window that expands with the right pointer and contracts with the left pointer.

### 3. What Makes This Problem Difficult?
The primary difficulty is code structure. If you try to manage the expansion and contraction using a single `while` loop with `if/else` statements, you will often leave valid elements unprocessed at the end of the array, requiring a second "drain" loop. The trick is to use a `for` loop to relentlessly expand the right boundary, and a nested `while` loop to relentlessly shrink the left boundary whenever the condition is met.

---

<details>
<summary><b>🧠 Pattern Recognition & Hints</b></summary>

### Pattern Recognition
"Contiguous subarray" + "minimal length" + "sum condition" = Dynamic Sliding Window.

### Hint 1
The `right` pointer aggressively expands the window. Let it iterate through the array using a standard `for` loop, adding elements to a running sum.

### Hint 2
The moment the `sum >= target`, the window is valid. But is it the *minimal* valid window? To find out, aggressively shrink the `left` pointer in a `while` loop, updating your minimal length each time, until the sum drops below the target.

</details>

---

<details>
<summary><b>💡 Core Insight</b></summary>
The dynamic sliding window acts like a caterpillar. The head (right pointer) stretches forward to find a valid food source (reaching the target). Once valid, the tail (left pointer) shrinks forward as much as possible to minimize the body length while still staying on the food. Because both the left and right pointers only move forward and never backward, every element is visited at most twice, guaranteeing an $O(N)$ time complexity.
</details>

---

<details open>
<summary><b>💻 Implementation</b></summary>

```cpp
class Solution {
public:
    int minSubArrayLen(int target, vector<int>& nums) {
        int n = nums.size();
        int left = 0;
        int sum = 0;
        int min_length = INT_MAX;
        
        // The right pointer aggressively expands the window
        for (int right = 0; right < n; right++) {
            sum += nums[right];
            
            // The moment the window is valid, aggressively shrink it from the left
            while (sum >= target) {
                // Update minimum length BEFORE shrinking
                min_length = min(min_length, right - left + 1);
                
                sum -= nums[left];
                left++;
            }
        }
        
        return min_length == INT_MAX ? 0 : min_length;
    }
};
```

</details>

---

<details>
<summary><b>🔍 Analysis & Edge Cases</b></summary>

### Correctness Proof
The `for` loop guarantees that every possible right boundary of a valid window is checked. The inner `while` loop guarantees that for every right boundary, the absolute tightest (most minimal) left boundary is found.

### Complexity
*   **Time:** $O(N)$ - Although there is a `while` loop inside a `for` loop, the `left` pointer only ever moves forward. It can move at most $N$ times across the entire program execution. Thus, $O(2N) = O(N)$.
*   **Space:** $O(1)$ - Only scalar variables are used.

### Edge Cases
*   Total sum of array is less than target: The inner `while` loop never triggers, `min_length` remains `INT_MAX`, and the function correctly returns `0`.
*   A single element is greater than or equal to target: The `while` loop triggers, calculates a length of `right - left + 1` (which is `1`), subtracts it, `left` moves past `right`, and the loop safely breaks.

</details>

---

### 🧠 What I Should Remember
**The Caterpillar Template.** For dynamic sliding windows, never use messy `if/else` blocks to control pointers. Use an outer `for(right)` loop to aggressively expand, and an inner `while(condition_is_valid)` loop to aggressively shrink from the `left`.
