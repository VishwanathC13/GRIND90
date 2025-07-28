### [567. Permutation in String](https://leetcode.com/problems/permutation-in-string/)

**Date:** July 28, 2025
**Pattern:** Fixed-Size Sliding Window with Frequency Map

---

### 1. Thought Process

- **Inputs & Outputs:** The input is two strings, `s1` and `s2`. The output is a boolean, `true` if `s2` contains a permutation of `s1` as a substring, and `false` otherwise.

- **Constraints:** The strings consist of lowercase English letters.

- **My Approach (Sliding Window with Frequency Arrays):**
  1.  This problem is asking if any anagram of `s1` exists as a substring in `s2`. This is a prime candidate for a sliding window approach.
  2.  The window size will be fixed to the length of `s1`.
  3.  To check if the substring in the window is a permutation of `s1`, I need to compare their character frequencies. The most efficient way to do this for lowercase English letters is to use two integer arrays of size 26 as frequency maps.
  4.  **Initialization:**
      - I'll create `array1` for the frequency map of `s1` and `array2` for the frequency map of the initial window in `s2`.
      - I'll populate both arrays by iterating from `0` to `s1.length() - 1`.
  5.  **Sliding the Window:**
      - After initialization, I'll iterate through `s2` from index `s1.length()` to the end. This loop will handle the sliding of the window.
      - Before the main loop starts, I'll do an initial check to see if the first window is already a match using `Arrays.equals(array1, array2)`.
      - In each iteration of the main loop:
        - First, I'll remove the character that is leaving the window from the left. I do this by decrementing its count in `array2`. The character is at `s2.charAt(i - x)`, where `x` is the length of `s1`.
        - Next, I'll add the new character that is entering the window from the right. I do this by incrementing its count in `array2`. The character is at `s2.charAt(i)`.
        - After updating the window's frequency map (`array2`), I'll compare it with `s1`'s frequency map (`array1`). If they are equal, I've found a permutation and can return `true` immediately.
  6.  **Final Check:** After the loop finishes, it's possible the very last window is a match. So, I need one final `Arrays.equals()` check before returning `false`.

---

### 2. Code

```java
class Solution {
    public boolean checkInclusion(String s1, String s2) {
        if (s1.length() > s2.length() || s2.length() == 0) return false;
        if (s1.length() == 0) return true;

        int x = s1.length(), y = s2.length();
        int[] array1 = new int[26];
        int[] array2 = new int[26];

        // Initialize the frequency maps for s1 and the first window of s2.
        for (int i = 0; i < x; i++) {
            array1[s1.charAt(i) - 'a']++;
            array2[s2.charAt(i) - 'a']++;
        }

        // Slide the window across the rest of s2.
        for (int i = x; i < y; i++) {
            if (Arrays.equals(array1, array2)) {
                return true;
            }
            // Remove the character leaving the window.
            array2[s2.charAt(i - x) - 'a']--;
            // Add the character entering the window.
            array2[s2.charAt(i) - 'a']++;
        }

        // Final check for the last window.
        if (Arrays.equals(array1, array2)) return true;
        else return false;
    }
}
```

---

### 3. Complexity Analysis

- **Time Complexity:** O(L1 + (L2 - L1)) which simplifies to O(L2), where L1 and L2 are the lengths of s1 and s2.

  - The initial loop runs L1 times.
  - The sliding window loop runs L2 - L1 times. Inside the loop, `Arrays.equals` takes O(26) time, which is constant.
  - The total time complexity is dominated by the length of the longer string, `s2`.

- **Space Complexity:** O(1) or O(26)
  - We use two integer arrays of a fixed size (26) to store character frequencies. This space does not scale with the input string lengths, so it's considered constant space.

---

### 4. Key Takeaways & Learnings

- This is a classic application of the fixed-size sliding window pattern.
- Using frequency arrays is a highly efficient method for checking if two strings are anagrams or permutations of each other, especially when the character set is limited (like lowercase English letters).
- The logic of updating the window's state in O(1) by adding one element and removing another is the core principle that makes this pattern so efficient.
