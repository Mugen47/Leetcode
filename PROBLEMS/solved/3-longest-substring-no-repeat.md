# 📝 3 - Longest Substring Without Repeating Characters

> [!NOTE]
> Link: [LeetCode 3](https://leetcode.com/problems/longest-substring-without-repeating-characters/)
> Difficulty: Medium
> Pattern: Sliding Window (Dynamic) + `std::unordered_map`

### 1. Constraints
*   `0 <= s.length <= 5 * 10^4`
*   `s` consists of English letters, digits, symbols and spaces.
*   Expected Time Complexity: $O(N)$

### 2. First Thoughts
We are looking for the longest contiguous sequence (substring) that satisfies a condition (no repeating characters). This is a dynamic sliding window. The window is valid as long as all characters inside it are unique. The moment `right` introduces a duplicate, `left` must jump forward to remove the old occurrence of that duplicate.

### 3. What Makes This Problem Difficult?
The naive approach is to use an `unordered_set` and crawl `left` forward one step at a time until the duplicate is removed. This is $O(N)$ in theory but slower in practice. The optimal insight is to store the *last seen index* of each character in an `unordered_map`, which allows `left` to **teleport** directly past the duplicate in $O(1)$ — skipping the slow crawl entirely.

---

<details>
<summary><b>🧠 Pattern Recognition & Hints</b></summary>

### Pattern Recognition
"Longest substring" + "without repeating characters" = Dynamic Sliding Window.
Needing to track *membership* of elements in a window + $O(1)$ lookup = `unordered_map` or `unordered_set`.

### Hint 1
What data structure tracks the last index of each character efficiently?

### Hint 2
When you find a duplicate at `right`, why can you directly set `left = map[s[right]] + 1` instead of crawling one by one?

### Hint 3
What happens if `map[s[right]] + 1` is less than `left`? The character was seen before, but its old position is outside the current window! You must guard against `left` moving backward.

</details>

---

<details>
<summary><b>💡 Core Insight</b></summary>

The `unordered_map` stores the **last known index** of each character. When `right` encounters a duplicate, `left` teleports to `map[s[right]] + 1`—directly past the old occurrence—in a single $O(1)$ jump. The `max(left, ...)` guard ensures `left` never moves backward, preventing stale out-of-window entries in the map from corrupting our window.

</details>

---

<details open>
<summary><b>💻 Implementation (Teleportation Approach)</b></summary>

```cpp
class Solution {
public:
    int lengthOfLongestSubstring(string s) {
        int n = s.size();
        unordered_map<char, int> last_seen; // char -> last seen index
        int left = 0;
        int max_length = 0;
        
        for (int right = 0; right < n; right++) {
            // If duplicate found AND it is inside our current window
            if (last_seen.find(s[right]) != last_seen.end()) {
                // Teleport left past the old occurrence (guarded against moving backward)
                left = max(left, last_seen[s[right]] + 1);
            }
            
            // Always update to the most recent index
            last_seen[s[right]] = right;
            
            // Window is now guaranteed valid — update answer
            max_length = max(max_length, right - left + 1);
        }
        
        return max_length;
    }
};
```

</details>

---

<details>
<summary><b>💻 Alternative Implementation (Deletion Approach)</b></summary>

```cpp
class Solution {
public:
    int lengthOfLongestSubstring(string s) {
        unordered_set<char> window;
        int left = 0;
        int max_length = 0;
        
        for (int right = 0; right < s.size(); right++) {
            // Crawl left until the duplicate is evicted
            while (window.count(s[right])) {
                window.erase(s[left]);
                left++;
            }
            window.insert(s[right]);
            max_length = max(max_length, right - left + 1);
        }
        
        return max_length;
    }
};
```

Both are $O(N)$, but the Teleportation approach is faster in practice.

</details>

---

<details>
<summary><b>🔍 Analysis & Edge Cases</b></summary>

### Correctness Proof
The `left` pointer only ever moves forward. The `max(left, ...)` guard ensures that stale map entries (characters seen before `left`) cannot corrupt the window. At every step, all characters in `[left, right]` are guaranteed unique.

### Complexity
*   **Time:** $O(N)$ — `right` iterates once. `left` teleports forward, never backward. Each character is processed exactly once.
*   **Space:** $O(min(N, C))$ — where `C` is the size of the character set (at most 128 for ASCII).

### Edge Cases
*   Empty string: `n = 0`, the loop never runs, returns `0`.
*   All unique characters: `left` never moves, returns `n`.
*   All same characters (e.g., `"aaaa"`): `left` teleports to `right` on every step, always returns `1`.

</details>

---

### 🧠 What I Should Remember
When a sliding window needs to track *membership* of characters:
*   Use `unordered_set` for **deletion-based** shrinking (simple but crawls).
*   Use `unordered_map<char, int>` for **teleportation-based** shrinking (optimal — jump directly past the duplicate using `left = max(left, map[ch] + 1)`).
