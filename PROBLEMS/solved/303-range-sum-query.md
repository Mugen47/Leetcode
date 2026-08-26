# 📝 303 - Range Sum Query — Immutable

> [!NOTE]
> Link: [LeetCode 303](https://leetcode.com/problems/range-sum-query-immutable/)
> Difficulty: Easy
> Pattern: Prefix Sum (Standard)

### Implementation

```cpp
class NumArray {
public:
    vector<int> prefix;

    NumArray(vector<int>& nums) {
        int n = nums.size();
        prefix.resize(n + 1, 0); // sentinel zero at index 0

        for (int i = 0; i < n; i++) {
            prefix[i + 1] = prefix[i] + nums[i];
        }
    }

    int sumRange(int left, int right) {
        return prefix[right + 1] - prefix[left]; // no if-check needed
    }
};
```

### Complexity
*   **Build:** $O(N)$ — one-time preprocessing.
*   **Query:** $O(1)$ — single subtraction.
*   **Space:** $O(N)$ — the prefix array.

### 🧠 What I Should Remember
Always use the `n+1` sized prefix array with `prefix[0] = 0` as a sentinel. This removes the need for `if (left == 0)` special-case logic and makes `sumRange(l, r) = prefix[r+1] - prefix[l]` universally correct.
