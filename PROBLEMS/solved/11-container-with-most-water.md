# 📝 11 - Container With Most Water

> [!NOTE]
> Link: [LeetCode 11](https://leetcode.com/problems/container-with-most-water/)
> Difficulty: Medium
> Pattern: Two Pointers (Opposite Direction)

### 1. Constraints
*   `n == height.length`
*   `2 <= n <= 10^5`
*   `0 <= height[i] <= 10^4`
*   Space complexity must be $O(1)$.
*   Expected Time Complexity: $O(N)$

### 2. First Thoughts
We want to maximize `Area = Width * Height`. The maximum possible width is when we take the very first and very last lines. To search for larger areas, we must move the pointers inward, which shrinks the width. Therefore, we only want to move a pointer if there is a chance of finding a taller line.

### 3. What Makes This Problem Difficult?
The difficulty is purely mathematical: proving to yourself that moving the taller line is useless, and therefore you can greedily discard the shorter line at every step without missing the global maximum.

---

<details>
<summary><b>🧠 Pattern Recognition & Hints</b></summary>

### Pattern Recognition
Searching for a pair of elements to maximize a function based on their distance and values. The distance naturally decreases as we move inwards, making Opposite Direction Two Pointers a perfect fit.

### Hint 1
Start with the maximum width (pointers at the ends). 

### Hint 2
The area is bottlenecked by the shorter line. If you move the taller line, the width decreases and the height cannot possibly exceed the shorter line. What does that tell you about which pointer to move?

</details>

---

<details>
<summary><b>💡 Core Insight</b></summary>
By always moving the pointer pointing to the shorter line, we safely eliminate the shorter line from all future combinations. It has already achieved the maximum possible width it could ever have, so pairing it with any inner line would result in a strictly smaller area.
</details>

---

<details>
<summary><b>🚀 Approaches</b></summary>

### Brute Force
*Time: O(N^2), Space: O(1)*
Nested loops checking every possible pair `i` and `j`. Fails on LeetCode due to Time Limit Exceeded.

### Optimal Approach
*Time: O(N), Space: O(1)*
Two Pointers closing inward, updating a running maximum and discarding the shorter wall.

</details>

---

<details open>
<summary><b>💻 Implementation</b></summary>

```cpp
class Solution {
public:
    int maxArea(vector<int>& height) {
        int ans = 0; // Area cannot be negative
        int n = height.size();
        int left = 0;
        int right = n - 1;
        
        while(left < right) {
            int width = right - left;
            int min_height = min(height[left], height[right]);
            
            ans = max(ans, width * min_height);
            
            if(height[left] < height[right]) {
                left++;
            } else {
                right--;
            }
        }
        return ans;
    }
};
```

</details>

---

<details>
<summary><b>🔍 Analysis & Edge Cases</b></summary>

### Correctness Proof
At every step, the container's width decreases by 1. The only way to increase the area is to increase the height. Since the height is bounded by the shorter line, moving the taller line can never increase the height. Thus, we must move the shorter line.

### Complexity
*   **Time:** $O(N)$ - Each line is evaluated at most once.
*   **Space:** $O(1)$ - Only using a few integer variables.

### Edge Cases
*   `height[left] == height[right]`: As proven, it does not matter which pointer moves. Moving either (or both) is mathematically safe because the area of the very next container is strictly bottlenecked by the remaining pointer.

</details>

---

### 🧠 What I Should Remember
When a mathematical formula relies on two variables from an array (like Width and Minimum Height), evaluate if one variable strictly decreases as you traverse. If it does, you can use Two Pointers to greedily optimize the other variable.
