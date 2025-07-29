### [1004. Max Consecutive Ones III](https://leetcode.com/problems/max-consecutive-ones-iii/)

**Date:** July 26, 2025
**Pattern:** Sliding Window

---

### 1. Thought Process

- **Inputs & Outputs:** The input is a binary array `nums` and an integer `k` representing the maximum number of zeros we can flip to ones. The output is an integer for the length of the longest contiguous subarray containing only ones (after flipping).

- **Constraints:** The array contains only 0s and 1s.

- **My Approach (Sliding Window):**
  1.  This problem asks for the _longest subarray_ that satisfies a condition, which is a perfect fit for the sliding window pattern.
  2.  I'll use two pointers: `j` for the start of the window (left pointer) and `i` for the end of the window (right pointer).
  3.  The variable `k` will be used to track the number of flips I have remaining.
  4.  I will iterate through the array with the right pointer `i` to expand the window.
  5.  **Expanding the Window:**
      - For each element `nums[i]`, if it's a `0`, I'll use one of my available flips by decrementing `k`.
  6.  **Shrinking the Window:**
      - After potentially using a flip, I'll check if `k` has become negative. If `k < 0`, it means my current window has too many zeros and is invalid.
      - To make the window valid again, I must shrink it from the left by moving the `j` pointer.
      - When I move `j`, I check if the element leaving the window (`nums[j]`) was a `0`. If it was, it means I'm regaining a flip, so I'll increment `k` back.
      - Then, I'll increment `j` to complete the shrink.
  7.  **Calculating the Result:**
      - The `while` loop maintains a "valid" window at all times (or fixes it immediately). The length of the window is `i - j`. As `i` always moves forward, the window only grows or slides. The maximum size of this window is maintained implicitly.
      - When the loop finishes, `i` will be at `nums.length`. The final window size, `i - j`, will represent the length of the longest valid subarray found.

---

### 2. Code

```java
class Solution {
    public int longestOnes(int[] nums, int k) {
        int j = 0; // Left pointer of the window
        int i = 0; // Right pointer of the window

        // Iterate with the right pointer to expand the window
        while(i < nums.length){
            // If we see a zero, use one of our k flips.
            if(nums[i] == 0) k--;

            // If k is negative, the window is invalid (too many zeros).
            if(k < 0){
                // If the element leaving the window is a zero, we regain a flip.
                if(nums[j] == 0){
                    k++;
                }
                // Shrink the window from the left.
                j++;
            }

            // Always expand the window to the right.
            i++;
        }
        // The final window size (i - j) is the answer.
        return i - j;
    }
}
```

---

### 3. Complexity Analysis

- **Time Complexity:** O(n)

  - Each pointer, `i` and `j`, traverses the array at most once from left to right. This results in a single pass and a linear time complexity.

- **Space Complexity:** O(1)
  - The algorithm uses only a few integer variables for the pointers and the counter `k`. The space required is constant and does not depend on the input array's size.

---

### 4. Key Takeaways & Learnings

- This is a great example of a sliding window where the window size itself represents the answer. We don't need a separate `max_length` variable because the window only expands or slides, never fully shrinks, thus preserving the maximum length found.
- The logic of using the `k` variable as a "budget" for zeros is very intuitive. When the budget goes negative, we must "pay back" by shrinking the window until it's valid again.
- This pattern is highly effective for problems asking for the longest subarray that satisfies a condition based on a limited number of "exceptions" (like flipping `k` zeros).
