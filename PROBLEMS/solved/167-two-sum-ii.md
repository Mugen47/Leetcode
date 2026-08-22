# 📝 167 - Two Sum II - Input Array Is Sorted

> [!NOTE]
> Link: [LeetCode 167](https://leetcode.com/problems/two-sum-ii-input-array-is-sorted/)
> Difficulty: Medium
> Pattern: Two Pointers (Opposite Direction)

### 1. Constraints
*   `2 <= numbers.length <= 3 * 10^4`
*   `-1000 <= numbers[i] <= 1000`
*   `numbers` is sorted in **non-decreasing order**.
*   Exactly one solution exists.
*   Space complexity must be $O(1)$.
*   **Expected Time Complexity:** $O(N)$ because $N = 3 \times 10^4$ means $O(N^2)$ brute force would do $9 \times 10^8$ operations, which might TLE (Time Limit Exceeded).

### 2. First Thoughts
Since the array is sorted and we are looking for a pair that sums to a target, we can place pointers at the extreme ends. If the sum is too large, we must decrease it by moving the right pointer left. If the sum is too small, we increase it by moving the left pointer right.

### 3. What Makes This Problem Difficult?
The main trick is simply recognizing that you don't need to check every combination. The sorted property provides a mathematical guarantee that allows us to eliminate elements safely. Also, remembering to return 1-based indices.

---

<details>
<summary><b>🧠 Pattern Recognition & Hints</b></summary>

### Pattern Recognition
"Sorted array" + "Find a pair" + "$O(1)$ space constraint" perfectly aligns with Opposite Direction Two Pointers.

### Hint 1
What is the largest possible sum? Where are those numbers located?

### Hint 2
If the current sum is greater than the target, which pointer is the only logical one to move?

</details>

---

<details>
<summary><b>💡 Core Insight</b></summary>
By moving the right pointer inward when the sum is too large, we mathematically eliminate the current right number from all future pairings, because pairing it with any smaller left number would only yield even smaller results. This shrinks the search space safely by $O(N)$ each step.
</details>

---

<details>
<summary><b>🚀 Approaches</b></summary>

### Brute Force
*Time: O(N^2), Space: O(1)*
Nested loops checking every possible pair `i` and `j`. Fails on LeetCode due to Time Limit Exceeded.

### Optimal Approach
*Time: O(N), Space: O(1)*
Two Pointers closing inward from both ends.

</details>

---

<details open>
<summary><b>💻 Implementation</b></summary>

```cpp
class Solution {
public:
    vector<int> twoSum(vector<int>& numbers, int target) {
        int left = 0;
        int right = numbers.size() - 1;
        
        while(left < right) {
            int current_sum = numbers[left] + numbers[right];
            
            if (current_sum == target) {
                return {left + 1, right + 1}; // 1-based indexing
            }
            else if (current_sum > target) {
                right--;
            }
            else {
                left++;
            }
        }
        
        return {-1, -1};
    }
};
```

</details>

---

<details>
<summary><b>🔍 Analysis & Edge Cases</b></summary>

### Correctness Proof
Because the array is sorted, every pointer movement permanently discards an element that cannot possibly be part of the solution, preventing us from missing the correct pair.

### Complexity
*   **Time:** $O(N)$ - In the worst case, each element is visited exactly once by either the left or right pointer.
*   **Space:** $O(1)$ - We only use two integer variables for pointers.

### Edge Cases
*   Negative numbers: The logic holds perfectly because `-5 < -2`.
*   Duplicate numbers: If `target = 6` and array is `[3, 3]`, pointers start at both 3s, sum is 6, and it immediately returns.

</details>

---

### 🧠 What I Should Remember
Whenever dealing with a **sorted** array and searching for combinations (sums, differences), Opposite Direction Two Pointers is almost always the answer to reduce complexity by one factor of $N$.
