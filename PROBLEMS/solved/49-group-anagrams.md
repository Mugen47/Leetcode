# 📝 49 - Group Anagrams

> [!NOTE]
> Link: [LeetCode 49](https://leetcode.com/problems/group-anagrams/)
> Difficulty: Medium
> Pattern: Hashing (Canonical Key Generation)

### Core Insight
Two strings are anagrams if and only if they produce the **same canonical key**. Group all strings by their canonical key using an `unordered_map<string, vector<string>>`.

The challenge is choosing the right key generation strategy:

| Approach | Key | Time per string | Total |
|----------|-----|-----------------|-------|
| Sort | Sorted string | $O(L \log L)$ | $O(N \times L \log L)$ |
| Frequency Array | `"1#0#0#0#1#...#1#0#"` | $O(L)$ | $O(N \times L)$ |

---

### Implementation 1: Sorting ($O(N \times L \log L)$)

```cpp
class Solution {
public:
    vector<vector<string>> groupAnagrams(vector<string>& strs) {
        unordered_map<string, vector<string>> m;

        for (int i = 0; i < strs.size(); i++) {
            string temp = strs[i];        // save original
            sort(strs[i].begin(), strs[i].end()); // sort in-place as key
            m[strs[i]].push_back(temp);   // no if/else needed — [] auto-creates vector
        }

        vector<vector<string>> ans;
        for (const auto& [key, value] : m) ans.push_back(value);
        return ans;
    }
};
```

---

### Implementation 2: Frequency Array ($O(N \times L)$) — Optimal

```cpp
class Solution {
public:
    vector<vector<string>> groupAnagrams(vector<string>& strs) {
        unordered_map<string, vector<string>> m;

        for (const string& word : strs) {
            vector<int> freq(26, 0);
            for (char c : word) freq[c - 'a']++;

            // Build key with '#' separator to prevent collisions
            // e.g. freq [1,0,0,0,1,0,...,1] → "1#0#0#0#1#0#...#1#0#"
            string key = "";
            for (int f : freq) key += to_string(f) + "#";

            m[key].push_back(word);
        }

        vector<vector<string>> ans;
        for (const auto& [key, value] : m) ans.push_back(value);
        return ans;
    }
};
```

### Why `#` separator?
Without it, `freq = [10, 0, ...]` → `"100..."` collides with `freq = [1, 0, 0, ...]` → also `"100..."`. The `#` separator makes each slot unambiguous.

### Complexity (Frequency Approach)
*   **Time:** $O(N \times L)$ — for each of $N$ strings, one $O(L)$ pass + $O(26)$ key build.
*   **Space:** $O(N \times L)$ — total characters stored across all map values.

### 🧠 What I Should Remember
When grouping items that share a *structural property* (anagram, same digit sum, etc.), always think **canonical key**. The key must be:
1. **Identical** for all equivalent items.
2. **Different** for non-equivalent items.
3. **Efficiently computable** — $O(L)$ preferred over $O(L \log L)$.
