# 📝 1 - Two Sum

> [!NOTE]
> Link: [LeetCode 1](https://leetcode.com/problems/two-sum/)
> Difficulty: Easy
> Pattern: Hashing (Complement Search)

### Core Insight
For each element `nums[i]`, instead of scanning the entire array for `target - nums[i]` in $O(N)$, store previously seen elements in a hash map. Check if the complement exists in $O(1)$.

**Order of operations is critical:** Check for complement BEFORE inserting `nums[i]`. This guarantees the map never contains the current index, making it impossible to pair an element with itself.

### Implementation

```cpp
class Solution {
public:
    vector<int> twoSum(vector<int>& nums, int target) {
        unordered_map<int, int> seen; // value -> index
        int n = nums.size();

        for (int i = 0; i < n; i++) {
            int complement = target - nums[i];
            if (seen.count(complement)) {
                return {seen[complement], i};
            }
            seen[nums[i]] = i; // insert AFTER checking
        }

        return {};
    }
};
```

### Complexity
*   **Time:** $O(N)$ — single pass; each lookup and insert is $O(1)$ average.
*   **Space:** $O(N)$ — map stores at most $N$ elements.

### 🧠 What I Should Remember
Two Sum is the archetype for all complement-search hashing problems. The template is: *check for complement → insert current*. Never reverse this order.
