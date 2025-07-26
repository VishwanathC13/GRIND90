### [3. Longest Substring Without Repeating Characters](https://leetcode.com/problems/longest-substring-without-repeating-characters/)

**Date:** July 23, 2025
**Pattern:** Sliding Window with HashSet

---

### 1. Thought Process

- **Inputs & Outputs:** The input is a string `s`. The output is an integer representing the length of the longest substring that does not contain any repeating characters.

- **Constraints:** The input can be an empty string.

- **My Approach (Sliding Window):**
  1.  A brute-force approach would check every possible substring for uniqueness, which would be very slow (O(n³)).
  2.  A much more efficient method is the **sliding window** pattern. I'll define a "window" (a substring) using two pointers, a left pointer `a_pointer` and a right pointer `b_pointer`.
  3.  I'll also use a `HashSet` to keep track of the unique characters currently inside my window. The HashSet provides O(1) average time complexity for adding, removing, and checking for existence.
  4.  I'll initialize both pointers to the start of the string (index 0) and a `max` length variable to 0.
  5.  I will expand the window by moving the `b_pointer` to the right as long as it's within the string's bounds.
  6.  **Decision Logic inside the loop:**
      - Check if the character at `s.charAt(b_pointer)` is already in the `hash_set`.
      - **If it's NOT in the set:** This means it's a unique character for the current window. I'll add it to the `hash_set`, increment `b_pointer` to expand the window, and then update `max` by comparing it with the current window size (`hash_set.size()`).
      - **If it IS in the set:** This means I have a repeating character. I need to shrink the window from the left until the duplicate is removed. I'll do this by removing the character at `s.charAt(a_pointer)` from the `hash_set` and then incrementing `a_pointer`. The loop will then continue, and in the next iteration, it will re-evaluate the character at `b_pointer`.
  7.  The loop continues until `b_pointer` reaches the end of the string. The `max` variable will hold the answer.

---

### 2. Code

```java
class Solution {
    public int lengthOfLongestSubstring(String s) {
        int a_pointer = 0;
        int b_pointer = 0;
        int max = 0;

        HashSet<Character> hash_set = new HashSet<>();

        while(b_pointer < s.length()){
            // If the character is not in our current window (the set)
            if(!hash_set.contains(s.charAt(b_pointer))){
                // Add it, expand the window, and update the max length.
                hash_set.add(s.charAt(b_pointer));
                b_pointer++;
                max = Math.max(hash_set.size(), max);
            } else {
                // If the character is a duplicate, shrink the window from the left.
                hash_set.remove(s.charAt(a_pointer));
                a_pointer++;
            }
        }
        return max;
    }
}
```

---

### 3. Complexity Analysis

- **Time Complexity:** O(n)

  - Each pointer, `a_pointer` and `b_pointer`, traverses the string at most `n` times. In the worst-case scenario, every character is visited twice (once by `b_pointer` and once by `a_pointer`), leading to O(2n), which simplifies to O(n).

- **Space Complexity:** O(k)
  - The space used is for the `HashSet`. In the worst case, the set will store `k` unique characters, where `k` is the number of unique characters in the input string. This could be up to `n` if all characters are unique, or a smaller constant (e.g., 26 for the English alphabet).

---

### 4. Key Takeaways & Learnings

- The sliding window pattern is extremely effective for problems involving finding optimal subarrays or substrings.
- Using a `HashSet` is the key to making the window's "contains" check efficient (O(1)). Without it, we would have to scan the substring every time, making the solution much slower.
- The logic of expanding the window when the condition is met and shrinking it when the condition is violated is a core concept of the dynamic sliding window pattern.
