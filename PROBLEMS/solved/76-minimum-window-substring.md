# 📝 76 - Minimum Window Substring

> [!NOTE]
> Link: [LeetCode 76](https://leetcode.com/problems/minimum-window-substring/)
> Difficulty: **Hard**
> Pattern: Sliding Window (Dynamic) + Character Frequency Map

### 1. Constraints
*   `m == t.length`, `n == s.length`
*   `1 <= m, n <= 10^5`
*   `s` and `t` consist of uppercase and lowercase English letters.
*   Expected Time Complexity: $O(N + M)$

### 2. First Thoughts
Find the shortest contiguous substring of `s` that contains all characters (with frequency) of `t`. This is a dynamic sliding window where:
*   The window is **valid** when it contains all required characters.
*   We expand `right` until valid, then shrink `left` to find the minimum.

The challenge: tracking character *frequency*, not just membership. A character appearing 3 times in `t` must appear at least 3 times in the window.

### 3. What Makes This Problem Difficult?
The character counting logic. The window can have *more* of a character than needed (surplus). We must distinguish between "genuinely needed" characters (frequency > 0 in map) and "surplus" characters (frequency ≤ 0 in map). The `sum` variable acts as a counter of *unsatisfied characters*, not just a total count.

---

<details>
<summary><b>🧠 Pattern Recognition & Hints</b></summary>

### Pattern Recognition
"Minimum window/substring" + "contains all characters of t" = Dynamic Sliding Window + Frequency Map.

### Hint 1
Build a frequency map of `t`. Initialize `sum = t.size()` to represent the total number of characters still needed.

### Hint 2
When `right` finds a character in the map AND `map[ch] > 0`, it is genuinely satisfying a need. Decrement `sum`. Always decrement `map[ch]` (even into negatives = tracking surplus).

### Hint 3
When `sum == 0`, the window is valid. Shrink from `left`. When `s[left]` is in the map, increment `map[s[left]]`. If `map[s[left]]` becomes > 0, we just lost a needed character — increment `sum` to invalidate the window.

</details>

---

<details>
<summary><b>💡 Core Insight</b></summary>

The `sum` variable doesn't track total character count — it tracks the count of **unsatisfied character slots**. Allowing `map` values to go *negative* (representing surplus supply) is the key trick that lets us manage over-satisfied characters cleanly without a separate tracking structure. When shrinking, only incrementing `sum` when `map[s[left]]` crosses back above 0 ensures we only invalidate the window when we've removed a *genuinely needed* character.

</details>

---

<details open>
<summary><b>💻 Implementation</b></summary>

```cpp
class Solution {
public:
    string minWindow(string s, string t) {
        int n = s.size();
        int m = t.size();
        
        if (n < m) return "";
        
        unordered_map<char, int> freq;
        int need = 0; // unsatisfied character slots
        
        for (char c : t) {
            freq[c]++;
            need++;
        }
        
        int left = 0;
        int min_left = 0;
        int min_len = INT_MAX;
        
        for (int right = 0; right < n; right++) {
            // Expand: absorb s[right] into window
            if (freq.count(s[right])) {
                if (freq[s[right]] > 0) need--; // genuinely needed
                freq[s[right]]--;               // track (goes negative = surplus)
            }
            
            // Shrink: while window is valid, try to minimize it
            while (need == 0) {
                if (right - left + 1 < min_len) {
                    min_left = left;
                    min_len = right - left + 1;
                }
                
                if (freq.count(s[left])) {
                    freq[s[left]]++;
                    if (freq[s[left]] > 0) need++; // lost a needed character
                }
                left++;
            }
        }
        
        return min_len == INT_MAX ? "" : s.substr(min_left, min_len);
    }
};
```

</details>

---

<details>
<summary><b>🔍 Analysis & Edge Cases</b></summary>

### Correctness Proof
`need` reaches 0 only when all character slots in `t` are satisfied. Allowing `freq` to go negative handles surplus characters without extra data structures. When shrinking, only incrementing `need` when `freq[ch]` crosses 0 from below ensures we only lose validity when we remove a *genuinely needed* character.

### Complexity
*   **Time:** $O(N + M)$ — Building the map is $O(M)$. `right` traverses `s` once, and `left` also traverses at most once.
*   **Space:** $O(C)$ — Where `C` is the character set size (at most 52 for upper+lower English letters).

### Edge Cases
*   `n < m`: Immediately return `""` — impossible to contain all of `t`.
*   `t` has duplicate characters (e.g., `t = "AA"`): The frequency map handles this correctly since `map['A'] = 2` and `need = 2`.
*   No valid window exists: `min_len` stays `INT_MAX`, return `""`.

</details>

---

### 🧠 What I Should Remember
The **Frequency + Need Counter** template:
1. Build `freq` map from `t`. Set `need = t.size()`.
2. Expand `right`: decrement `need` only when `freq[ch] > 0`. Always decrement `freq[ch]`.
3. Shrink `left` while `need == 0`: update answer, then increment `need` only when `freq[ch]` crosses back above 0.
