### [438. Find All Anagrams in a String](https://leetcode.com/problems/find-all-anagrams-in-a-string/)

**Date:** July 28, 2025
**Pattern:** Fixed-Size Sliding Window with Optimized Frequency Count

---

### 1. Thought Process

- **Inputs & Outputs:** The input is a string `s` and a non-empty string `p`. The output is a `List<Integer>` containing all the start indices of `p`'s anagrams in `s`.

- **Constraints:** The strings consist of lowercase English letters.

- **My Approach (Optimized Sliding Window):**
  1.  This problem is very similar to "Permutation in String," but instead of returning `true`, I need to return the starting indices of all occurrences.
  2.  I will use a sliding window with a fixed size equal to the length of `p`.
  3.  Instead of using two frequency maps and comparing them, this approach uses a more optimized technique with a single frequency map and a counter variable.
  4.  **Initialization:**
      - Create a single frequency map (`char_count`) for the pattern string `p`.
      - Initialize a `count` variable to `p.length()`. This `count` will represent the number of characters from `p` we still need to find in our current window.
      - Initialize two pointers, `left` and `right`, to 0.
  5.  **Sliding the Window:**
      - I'll iterate with the `right` pointer to expand the window.
      - For the character `s.charAt(right)`, I'll decrement its required count in `char_count`. If the count of that character _before_ decrementing was `1` or more, it means it's a character we were looking for, so I'll decrement `count`.
      - **Checking for Anagrams:** When `count` reaches `0`, it means the current window contains all the necessary characters for an anagram. At this point, I'll add the `left` pointer's index to my `result` list.
      - **Shrinking the Window:** When the window size (`right - left`) reaches the length of `p`, I need to shrink it from the left. I'll take the character `s.charAt(left)` and increment its count back in the `char_count` map. If the count of that character _before_ incrementing was `0` or more, it means it was a character that was part of the required anagram, so I must increment `count` because I now need to find it again. Finally, I'll increment `left`.
  6.  This process of expanding, checking, and shrinking continues until the `right` pointer reaches the end of `s`.

---

### 2. Code

```java
class Solution {
    public List<Integer> findAnagrams(String s, String p) {
        List<Integer> result = new ArrayList<>();

        if (s.length() == 0 || s == null) return result;

        int[] char_count = new int[26];
        for (char c : p.toCharArray()) {
            char_count[c - 'a']++;
        }

        int left = 0;
        int right = 0;
        int count = p.length();

        while (right < s.length()) {
            // Expand the window from the right.
            // If the character at 'right' is one we need, decrement count.
            if (char_count[s.charAt(right++) - 'a']-- >= 1) count--;

            // If count is 0, we found an anagram.
            if (count == 0) result.add(left);

            // If the window is now the size of p, shrink it from the left.
            // If the character leaving was one we needed, increment count.
            if (right - left == p.length() && char_count[s.charAt(left++) - 'a']++ >= 0) count++;
        }

        return result;
    }
}
```

---

### 3. Complexity Analysis

- **Time Complexity:** O(L2), where L2 is the length of string `s`.

  - We iterate through the string `s` with the `right` and `left` pointers exactly once. All operations inside the loop are O(1).

- **Space Complexity:** O(1) or O(26)
  - We use a single integer array of a fixed size (26) for the frequency map. This space does not scale with the input string lengths, so it's considered constant space.

---

### 4. Key Takeaways & Learnings

- This is a more advanced and optimized version of the sliding window pattern for anagrams.
- Using a `count` variable to track how many required characters are left to find is more efficient than comparing two full frequency arrays in every step.
- The conditions `char_count[...]-- >= 1` and `char_count[...]++ >= 0` are subtle but brilliant. They correctly handle cases where the window might contain extra characters that are not in `p`.
- This solution elegantly combines the expanding and shrinking logic into a very compact form.
