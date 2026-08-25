# 📝 643 - Maximum Average Subarray I

> [!NOTE]
> Link: [LeetCode 643](https://leetcode.com/problems/maximum-average-subarray-i/)
> Difficulty: Easy
> Pattern: Sliding Window (Fixed Size)

### 1. Constraints
*   `n == nums.length`
*   `1 <= k <= n <= 10^5`
*   `-10^4 <= nums[i] <= 10^4`
*   Expected Time Complexity: $O(N)$

### 2. First Thoughts
Since the problem explicitly asks for a contiguous subarray of a fixed length `k`, this is the most textbook example of a Fixed-Size Sliding Window. Doing a nested loop to calculate the sum of every subarray of size `k` would take $O(N \times K)$, which will Time Limit Exceed. We must use the sliding window to do it in $O(N)$.

### 3. What Makes This Problem Difficult?
The algorithm itself is simple, but writing it *cleanly and optimally* is the challenge. If you do the division `/ k` on every step, or if you put an `if (i < k)` check inside your main loop, you are adding unnecessary CPU cycles. The trick is to separate the initialization of the window from the sliding of the window, and delay division until the very end.

---

<details>
<summary><b>🧠 Pattern Recognition & Hints</b></summary>

### Pattern Recognition
"Contiguous subarray" + "length is equal to k" = Fixed-Size Sliding Window.

### Hint 1
First, write a loop that just adds up the first `k` elements. Save this as your `max_sum`.

### Hint 2
Next, write a loop starting from index `k`. To slide the window, add the new element entering the window (`nums[i]`) and subtract the old element leaving the window (`nums[i - k]`).

</details>

---

<details>
<summary><b>💡 Core Insight</b></summary>
A moving window of size `K` only changes by exactly two elements at a time: one enters, one leaves. The other `K-2` elements stay exactly the same. By subtracting the outgoing element and adding the incoming element, we calculate the new sum in $O(1)$ constant time, reducing the total algorithm to $O(N)$.
</details>

---

<details open>
<summary><b>💻 Implementation</b></summary>

```cpp
class Solution {
public:
    double findMaxAverage(vector<int>& nums, int k) {
        double max_sum = 0;
        double current_sum = 0;
        
        // 1. Build the initial window
        for (int i = 0; i < k; i++) {
            current_sum += nums[i];
        }
        
        max_sum = current_sum;
        
        // 2. Slide the window
        for (int i = k; i < nums.size(); i++) {
            current_sum = current_sum - nums[i - k] + nums[i];
            max_sum = max(max_sum, current_sum);
        }
        
        // 3. Divide exactly once at the end
        return max_sum / k;
    }
};
```

</details>

---

<details>
<summary><b>🔍 Analysis & Edge Cases</b></summary>

### Correctness Proof
By separating the initialization from the sliding phase, we guarantee that the window size is always exactly `k`. Updating the maximum sum at every step guarantees we find the global maximum. Delaying division is mathematically safe because dividing by a positive constant `k` preserves the ordering of sums (i.e., if Sum A > Sum B, then A/k > B/k).

### Complexity
*   **Time:** $O(N)$ - We iterate through the array exactly once.
*   **Space:** $O(1)$ - We only use two `double` variables.

### Edge Cases
*   `k == n`: The first loop consumes the entire array. The second loop never runs. Returns the average of the whole array perfectly.
*   Negative numbers: `max_sum` correctly tracks negative numbers because we initialize it to the sum of the first window, not to `0` or `INT_MIN`.

</details>

---

### 🧠 What I Should Remember
1. **Never use an `if(i < k)` inside a sliding window loop.** Always use two separate loops: one to build the first window, and one to slide it. It is cleaner and faster.
2. **Delay division.** Division is computationally expensive. Just track the maximum *sum*, and only divide when you return the final answer.
