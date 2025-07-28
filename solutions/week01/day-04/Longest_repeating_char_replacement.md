### [424. Longest Repeating Character Replacement](https://leetcode.com/problems/longest-repeating-character-replacement/)

**Date:** July 27, 2025
**Pattern:** Sliding Window with Frequency Map

---

### 1. Thought Process

- **Inputs & Outputs:** The input is a string `s` and an integer `k`. The output is an integer representing the length of the longest substring containing the same letter you can get after performing at most `k` character replacements.

- **Constraints:** The string consists of uppercase English letters.

- **My Approach (Sliding Window):**
  1.  This problem asks for the longest substring that satisfies a certain property, which is a strong indicator for the sliding window pattern.
  2.  I'll define a window with `window_start` and `window_end` pointers.
  3.  To check the property of the window, I need to know the frequency of each character within it. An integer array `char_counts` of size 26 is a perfect frequency map for this.
  4.  **The Core Logic (Window Validity):**
      - A window is "valid" if the number of characters we need to replace is less than or equal to `k`.
      - The number of characters to replace is `(length of the window) - (count of the most frequent character in the window)`.
      - So, the condition is: `(window_end - window_start + 1) - max_count <= k`.
  5.  I'll iterate through the string with `window_end` to expand the window. In each step:
      - I'll increment the count of the character at `s.charAt(window_end)`.
      - I'll update `max_count`, which stores the highest frequency of any single character seen so far in the current window.
  6.  **Shrinking the Window:**
      - After expanding, I'll check if the window has become invalid using a `while` loop: `while (window_end - window_start + 1 - max_count > k)`.
      - If it's invalid, I need to shrink it from the left. I'll decrement the count of the character at `s.charAt(window_start)` and then increment `window_start`.
      - A subtle but crucial point: I don't need to recalculate `max_count` when shrinking. We only care about the `max_count` for the largest valid window, so we can maintain the historical max. The window will only grow again when we find a character with an even higher frequency.
  7.  After each expansion and potential shrink, the window is guaranteed to be valid. I'll update my `max_length` with the size of this current valid window (`window_end - window_start + 1`).
  8.  After the loop finishes, `max_length` will hold the answer.

---

### 2. Code

```java
class Solution {
    public int characterReplacement(String s, int k) {
        int N = s.length();
        int[] char_counts = new int[26];

        int window_start = 0;
        int max_length = 0;
        int max_count = 0; // Tracks the count of the most frequent char in the window

        for(int window_end = 0; window_end < N; window_end++){
            // Increment count for the new character in the window.
            char_counts[s.charAt(window_end) - 'A']++;
            // Update the max frequency count.
            max_count = Math.max(max_count, char_counts[s.charAt(window_end) - 'A']);

            // Check if the current window is invalid.
            // (Window size - max frequency) > k means we need more than k replacements.
            while(window_end - window_start + 1 - max_count > k){
                // Shrink the window from the left.
                char_counts[s.charAt(window_start) - 'A']--;
                window_start++;
            }

            // The current window is valid, so update max_length.
            max_length = Math.max(max_length, window_end - window_start + 1);
        }
        return max_length;
    }
}
```

---

### 3. Complexity Analysis

- **Time Complexity:** O(n)

  - The `window_end` pointer iterates through the string once. The `window_start` pointer also moves forward at most `n` times. This gives a total of O(2n) operations, which simplifies to O(n).

- **Space Complexity:** O(1) or O(26)
  - The `char_counts` array has a fixed size of 26, which does not change with the input string's length. Therefore, the space complexity is constant.

---

### 4. Key Takeaways & Learnings

- This is a challenging sliding window problem where the validity condition is more complex than usual.
- The key insight is that `window_length - max_count` represents the number of "minority" characters in the window, which is the number of characters that need to be replaced.
- The optimization of not needing to re-calculate `max_count` precisely when shrinking the window is a common pattern in these types of problems. We are only interested in expanding the window, so we only need to care about the all-time max frequency encountered.
