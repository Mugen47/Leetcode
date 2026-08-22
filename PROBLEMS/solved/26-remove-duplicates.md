# 📝 26 - Remove Duplicates from Sorted Array

> [!NOTE]
> Link: [LeetCode 26](https://leetcode.com/problems/remove-duplicates-from-sorted-array/)
> Difficulty: Easy
> Pattern: Two Pointers (Same Direction / Read & Write)

### 1. Constraints
*   `1 <= nums.length <= 3 * 10^4`
*   `-100 <= nums[i] <= 100`
*   `nums` is sorted in **non-decreasing** order.
*   In-place modification with $O(1)$ extra space.
*   Expected Time Complexity: $O(N)$

### 2. First Thoughts
Since the array is sorted, all duplicate elements are grouped together. To modify the array in-place without overwriting data we haven't processed yet, we can use two pointers moving in the same direction. One pointer reads the data, and the other pointer writes the valid data.

### 3. What Makes This Problem Difficult?
The main challenge is understanding array overwrites. If we try to `erase()` elements from the middle of a `std::vector`, it takes $O(N)$ time per deletion, resulting in an $O(N^2)$ brute force approach. The trick is to just overwrite the values and ignore the garbage left at the end.

---

<details>
<summary><b>🧠 Pattern Recognition & Hints</b></summary>

### Pattern Recognition
"In-place modification" + "Array processing" almost always implies Same Direction Two Pointers (Read/Write pointers).

### Hint 1
You don't actually need to delete the duplicates. You just need to move the unique elements to the front.

### Hint 2
If you have a `fast` pointer scanning the array and a `slow` pointer keeping track of the last unique element, what condition tells the `fast` pointer that it has found something new?

</details>

---

<details>
<summary><b>💡 Core Insight</b></summary>
The `slow` pointer acts as a safe boundary. Everything at and behind `slow` is guaranteed to be unique and sorted. The `fast` pointer explores the unknown territory ahead. Because `fast` always moves at least as fast as `slow`, we are mathematically guaranteed that `slow` will never overwrite an element that `fast` hasn't already processed.
</details>

---

<details open>
<summary><b>💻 Implementation</b></summary>

```cpp
class Solution {
public:
    int removeDuplicates(vector<int>& nums) {
        int n = nums.size();
        if (n == 0) return 0; // Edge case safety
        
        int slow = 0;
        for (int fast = 1; fast < n; fast++) {
            if (nums[slow] != nums[fast]) {
                slow++;
                nums[slow] = nums[fast];
            }
        }
        return slow + 1; // Return the count of unique elements
    }
};
```

</details>

---

<details>
<summary><b>🔍 Analysis & Edge Cases</b></summary>

### Correctness Proof
The first element is trivially unique. For every subsequent element, if it matches the last unique element (`nums[slow]`), it's a duplicate and is ignored. If it differs, it must be a new unique element (due to the array being sorted), so we advance the `slow` boundary and record it.

### Complexity
*   **Time:** $O(N)$ - The `fast` pointer visits each element exactly once.
*   **Space:** $O(1)$ - Only using two integer variables.

### Edge Cases
*   Empty array (`nums.size() == 0`): Must return 0 immediately (though constraints say length is at least 1, it's good practice).
*   Array with all identical elements (`[1, 1, 1]`): `fast` reaches the end, `slow` stays at 0, returns 1.
*   Array with all unique elements (`[1, 2, 3]`): `fast` and `slow` move in lockstep, `slow` increments every time.

</details>

---

### 🧠 What I Should Remember
When you need to filter or compress an array in-place, use a **Fast (Reader)** pointer and a **Slow (Writer)** pointer. The Writer only moves when the Reader validates the condition.
