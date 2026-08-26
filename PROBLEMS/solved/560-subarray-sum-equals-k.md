# 📝 560 - Subarray Sum Equals K

> [!NOTE]
> Link: [LeetCode 560](https://leetcode.com/problems/subarray-sum-equals-k/)
> Difficulty: Medium
> Pattern: Prefix Sum + `std::unordered_map` (Frequency Counting)

### 1. Core Insight
`sum(l, r) = k` ⟺ `prefix[r+1] - prefix[l] = k` ⟺ `prefix[l] = prefix[r+1] - k`

For each new prefix sum, ask: *"How many times have we already seen the value `prefixSum - k`?"*
An `unordered_map` answers this in $O(1)$.

### 2. The Critical Sentinel

```cpp
mpp[0] = 1; // MUST be initialized before the loop
```

Without this, subarrays starting at index 0 (where `prefixSum == k`) are silently missed.

### 3. Implementation

```cpp
class Solution {
public:
    int subarraySum(vector<int>& nums, int k) {
        unordered_map<int, int> freq;
        freq[0] = 1; // sentinel: prefix sum of 0 seen once before array starts
        int prefix_sum = 0, count = 0;

        for (int x : nums) {
            prefix_sum += x;
            count += freq[prefix_sum - k]; // valid left boundaries
            freq[prefix_sum]++;
        }
        return count;
    }
};
```

### Complexity
*   **Time:** $O(N)$ — single pass.
*   **Space:** $O(N)$ — map stores at most $N$ distinct prefix sums.

### 🧠 What I Should Remember
**Prefix Sum + HashMap = Count subarrays satisfying a condition.**
The pattern is always: compute running prefix sum → look up `prefix - target` in the map → update the map.
`map[0] = 1` is non-negotiable.
