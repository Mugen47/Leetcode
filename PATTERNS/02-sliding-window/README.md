# 🪟 Pattern 02: Sliding Window

> [!NOTE]
> **Category:** Arrays & Strings (Subarrays/Substrings)
> **Prerequisites:** Basic C++, `std::vector`, `std::string`, `std::max`, `std::min`
> **Difficulty:** Medium
> **Goal:** Track a continuous sequence of elements to optimize a condition in $O(N)$ time.

---

## 🎯 What Problem Does This Solve?

The **Sliding Window** pattern is a highly specific evolution of Two Pointers. It is used almost exclusively when a problem asks you to find or optimize a **continuous sequence** (a subarray or a substring).

If a problem asks for "a pair of numbers", you use Two Pointers.
If a problem asks for a **"continuous sequence of numbers"** (e.g., "Find the maximum sum of any contiguous subarray of size `k`"), you use a Sliding Window.

### The Naive Approach ($O(N^2)$ or $O(N^K)$)
If we want to find the sum of every subarray of size 3, the brute-force way is to start at index 0, add the next 3 numbers. Then start at index 1, add the next 3 numbers. Then index 2...
This recalculates the shared overlapping elements every single time, wasting massive amounts of CPU cycles.

### The Sliding Window Approach ($O(N)$)
Imagine a physical window frame that exactly covers 3 numbers. 
1. We calculate the sum of the numbers inside the window once.
2. To move the window forward by one step, we simply **subtract** the element that fell out of the left side of the frame, and **add** the new element that entered the right side of the frame.
3. This turns an $O(N \times K)$ loop into a brilliant $O(N)$ single pass.

---

## 🧩 The Two Archetypes

There are exactly two types of Sliding Window problems. If you can identify which one you are looking at, the code writes itself.

### 1. Fixed-Size Window
*   **The Scenario:** The problem explicitly tells you the size of the window (e.g., "Find the max sum of a subarray of size **K**").
*   **The Logic:** The distance between your `left` and `right` pointers is permanently locked to `K`. When `right` moves forward by 1, `left` MUST move forward by 1 to maintain the rigid window size.

### 2. Dynamically Sizing Window
*   **The Scenario:** The problem asks you to find the *best size* based on a condition (e.g., "Find the **longest** subarray whose sum is less than `X`", or "Find the **shortest** substring containing all characters").
*   **The Logic:** The window acts like a caterpillar. The `right` pointer aggressively expands to absorb elements until a condition is broken. Then, the `left` pointer aggressively shrinks the window until the condition is fixed.

---

## 💻 Template (Fixed-Size Window)

<details>
<summary><b>Reveal Template</b></summary>

```cpp
#include <vector>
#include <algorithm>

using namespace std;

int fixedSlidingWindow(const vector<int>& arr, int k) {
    int max_sum = 0;
    int current_window_sum = 0;
    
    // 1. Initialize the first window
    for (int i = 0; i < k; i++) {
        current_window_sum += arr[i];
    }
    max_sum = current_window_sum;
    
    // 2. Slide the window across the array
    for (int i = k; i < arr.size(); i++) {
        // Add the new element on the right, remove the old element on the left
        current_window_sum += arr[i] - arr[i - k]; 
        
        // Update the answer
        max_sum = max(max_sum, current_window_sum);
    }
    
    return max_sum;
}
```
</details>

---

## 💻 Template (Dynamic Window)

<details>
<summary><b>Reveal Template</b></summary>

```cpp
#include <vector>
#include <algorithm>

using namespace std;

int dynamicSlidingWindow(const vector<int>& arr, int target) {
    int max_length = 0;
    int current_sum = 0;
    int left = 0;
    
    // Expand the window with the right pointer
    for (int right = 0; right < arr.size(); right++) {
        current_sum += arr[right];
        
        // If the window is INVALID, shrink it from the left until it becomes valid again
        while (current_sum >= target) {
            current_sum -= arr[left];
            left++;
        }
        
        // Window is now VALID. Update the answer based on the current window size
        max_length = max(max_length, right - left + 1);
    }
    
    return max_length;
}
```
</details>

---

## ⚠️ Common Traps
1.  **Off-by-one errors on window size:** The number of elements between `left` and `right` (inclusive) is always `right - left + 1`. Don't forget the `+1`!
2.  **Updating the answer at the wrong time:** In a dynamic window, if you update `max_length` *inside* the `while` loop, you are recording the size of an INVALID window. Always update your answer *after* the `while` loop when the window is guaranteed to be valid.
