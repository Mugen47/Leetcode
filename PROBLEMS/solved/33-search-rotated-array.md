# 📝 33 - Search in Rotated Sorted Array

> [!NOTE]
> Link: [LeetCode 33](https://leetcode.com/problems/search-in-rotated-sorted-array/)
> Difficulty: Medium
> Pattern: Binary Search (Modified Array)

### Core Insight
In a rotated sorted array, when you compute `mid`, **one of the two halves is always perfectly sorted**. Check which half is sorted using `nums[start] <= nums[mid]`. Then check in $O(1)$ whether `target` falls within that sorted half's range. This allows you to eliminate half the array at each step, maintaining $O(\log N)$.

> [!IMPORTANT]
> Finding the pivot first with a linear scan is $O(N)$ and violates the constraint. The single-pass binary search does not need to find the pivot at all.

### Implementation

```cpp
class Solution {
public:
    int search(vector<int>& nums, int target) {
        int start = 0;
        int end = nums.size() - 1;

        while (start <= end) {
            int mid = start + (end - start) / 2;

            if (nums[mid] == target) return mid;

            // Left half [start, mid] is sorted
            if (nums[start] <= nums[mid]) {
                if (nums[start] <= target && target < nums[mid])
                    end = mid - 1;   // target in left sorted half
                else
                    start = mid + 1; // target in right half
            }
            // Right half [mid, end] is sorted
            else {
                if (nums[mid] < target && target <= nums[end])
                    start = mid + 1; // target in right sorted half
                else
                    end = mid - 1;   // target in left half
            }
        }

        return -1;
    }
};
```

### Complexity
*   **Time:** $O(\log N)$ — halves the search space at every step.
*   **Space:** $O(1)$

### 🧠 What I Should Remember
Rotated array → one half is always sorted. Use `nums[start] <= nums[mid]` to identify which half. Use the sorted half's boundary values to decide where target lives. No pivot scan needed.
