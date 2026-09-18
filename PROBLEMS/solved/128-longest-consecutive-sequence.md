# 📝 128 - Longest Consecutive Sequence

> [!NOTE]
> Link: [LeetCode 128](https://leetcode.com/problems/longest-consecutive-sequence/)
> Difficulty: Medium
> Pattern: Hashing (`unordered_set` + Smart Start Condition)

### Core Insight
Put all numbers in an `unordered_set` for $O(1)$ lookup. Then for each number, only begin counting a sequence if it is a **sequence starter** — i.e., `num - 1` is NOT in the set.

This avoids redundant work: if we counted from every element, elements in the middle of sequences would re-count the same sequence, giving $O(N^2)$. By only starting from the leftmost element of each sequence, every element is visited **at most twice**: once in the outer loop, once in the `while` loop.

### Implementation

```cpp
class Solution {
public:
    int longestConsecutive(vector<int>& nums) {
        if (nums.empty()) return 0;

        unordered_set<int> s(nums.begin(), nums.end()); // O(N) build
        int ans = 1;

        for (int num : nums) {
            // Only start counting from the beginning of a sequence
            if (!s.count(num - 1)) {
                int curr = num;
                int length = 1;

                while (s.count(curr + 1)) {
                    curr++;
                    length++;
                }

                ans = max(ans, length);
            }
        }
        return ans;
    }
};
```

### Complexity
*   **Time:** $O(N)$ — Each number is the start of at most one sequence. The inner `while` loop's total iterations across the entire outer loop ≤ $N$.
*   **Space:** $O(N)$ — the hash set.

### 🧠 What I Should Remember
The "smart start" condition `if (!s.count(num - 1))` is the key that converts an $O(N^2)$ scan into $O(N)$. Whenever you are "expanding outward" from elements, ask yourself: *"Can I identify which elements are valid starting points to avoid redundant expansions?"*
