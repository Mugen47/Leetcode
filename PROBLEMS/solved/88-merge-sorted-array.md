# 📝 88 - Merge Sorted Array

> [!NOTE]
> Link: [LeetCode 88](https://leetcode.com/problems/merge-sorted-array/)
> Difficulty: Easy
> Pattern: Two Pointers (Merging / Right-to-Left)

### 1. Constraints
*   `nums1.length == m + n`
*   `nums2.length == n`
*   `0 <= m, n <= 200`
*   `-10^9 <= nums1[i], nums2[j] <= 10^9`
*   In-place modification with $O(1)$ extra space.
*   Expected Time Complexity: $O(m + n)$

### 2. First Thoughts
We need to merge two sorted arrays into one. Normally, this is done left-to-right. However, `nums1` contains the result buffer at its *end*. If we try to merge left-to-right, placing a small element from `nums2` into `nums1[0]` might overwrite a valid element in `nums1` that we haven't processed yet, requiring a massive $O(N)$ shift operation.

### 3. What Makes This Problem Difficult?
The trick is reversing your thinking. The empty space is at the back. Therefore, the safest place to write data is at the back. To maintain the sorted order when writing at the back, we must compare the *largest* elements, not the smallest elements.

---

<details>
<summary><b>🧠 Pattern Recognition & Hints</b></summary>

### Pattern Recognition
Merging two sorted arrays is the textbook definition of the Merging Two Pointers pattern. The "in-place with empty space at the end" constraint turns it into the Right-to-Left sub-pattern.

### Hint 1
Where is the empty space? Since it's at the end, what if we started our merging process at the end instead of the beginning?

### Hint 2
If we start at the end, we need to place the largest numbers first. How do you find the largest number between two sorted arrays?

</details>

---

<details>
<summary><b>💡 Core Insight</b></summary>
By placing pointers at the *ends* of the valid data (`m - 1` and `n - 1`) and a write pointer at the absolute end of `nums1` (`m + n - 1`), we can compare the largest elements. Whichever is larger gets written to the back. Because we are consuming the empty space first, we are mathematically guaranteed to never overwrite an element in `nums1` before we have safely processed it.
</details>

---

<details open>
<summary><b>💻 Implementation</b></summary>

```cpp
class Solution {
public:
    void merge(vector<int>& nums1, int m, vector<int>& nums2, int n) {
        // We use m and n directly as our pointers (decrementing them)
        int write_ptr = m + n - 1;
        
        while (m > 0 && n > 0) {
            if (nums1[m - 1] > nums2[n - 1]) {
                nums1[write_ptr] = nums1[m - 1];
                m--;
            } else {
                nums1[write_ptr] = nums2[n - 1];
                n--;
            }
            write_ptr--;
        }
        
        // If nums2 still has elements, copy them over.
        // (If nums1 has elements left, they are already in the correct place!)
        while (n > 0) {
            nums1[write_ptr] = nums2[n - 1];
            n--;
            write_ptr--;
        }
    }
};
```

</details>

---

<details>
<summary><b>🔍 Analysis & Edge Cases</b></summary>

### Correctness Proof
At any step, the `write_ptr` is placing the absolute maximum of the remaining elements at the back of the array. The `write_ptr` moves leftward. It will only reach index `i` of `nums1` after `(m + n - 1) - i` elements have been processed. Since we process 1 element per step, `write_ptr` can never overtake the read pointer `m - 1`, making overwrites impossible.

### Complexity
*   **Time:** $O(m + n)$ - Each element from both arrays is evaluated and written exactly once.
*   **Space:** $O(1)$ - Entirely in-place.

### Edge Cases
*   `nums1` is initially empty (`m = 0`): The first loop skips, and the second loop perfectly copies all of `nums2` into `nums1`.
*   `nums2` is empty (`n = 0`): Both loops skip. `nums1` remains unchanged, which is already correct.

</details>

---

### 🧠 What I Should Remember
Whenever you need to merge or shift data in-place into an array that has empty space at the **back**, always use **Right-to-Left** pointers. Writing into empty space is always safe.
