### [209. Minimum Size Subarray Sum](https://leetcode.com/problems/minimum-size-subarray-sum/)

**Date:** July 27, 2025
**Pattern:** Dynamic Sliding Window

---

### 1. Thought Process

- **Inputs & Outputs:** The input is an integer `target` and an array of positive integers `nums`. The output is the minimal length of a contiguous subarray whose sum is greater than or equal to `target`. If no such subarray exists, the output is 0.

- **Constraints:** The numbers in the array are positive.

- **My Approach (Sliding Window):**
  1.  A brute-force approach would check the sum of every possible contiguous subarray, which would be O(n²), likely too slow.
  2.  A more optimal approach is the **dynamic sliding window**. I'll use two pointers: a `left` pointer to mark the start of the window, and the loop's iterator `i` will serve as the right pointer.
  3.  I'll initialize `result` to the largest possible integer value (`Integer.MAX_VALUE`). This is a standard way to track a minimum value, as any valid length will be smaller than it. I'll also initialize the window's sum, `val_sum`, to 0.
  4.  I'll iterate through the `nums` array with the right pointer `i`, expanding the window by adding `nums[i]` to `val_sum`.
  5.  **The Core Logic (Shrinking the window):**
      - As soon as `val_sum` becomes greater than or equal to the `target`, I have found a valid subarray.
      - I'll update my `result` with the minimum length found so far: `result = Math.min(result, i + 1 - left)`. The length is `right - left + 1`.
      - Now, I need to see if I can find an even _shorter_ valid subarray. I'll shrink the window from the left by subtracting `nums[left]` from `val_sum` and incrementing `left`.
      - I'll keep shrinking the window in a `while` loop as long as the `val_sum` remains greater than or equal to the `target`, updating the `result` each time.
  6.  After the main loop finishes, I need to check if `result` was ever updated. If it's still `Integer.MAX_VALUE`, it means no valid subarray was ever found, so I should return 0. Otherwise, I return the `result`.

---

### 2. Code

```java
class Solution {
    public int minSubArrayLen(int target, int[] nums) {
        int result = Integer.MAX_VALUE;
        int left = 0;
        int val_sum = 0;

        // 'i' acts as the right pointer of the window.
        for(int i = 0; i < nums.length; i++){
            // Expand the window by adding the right element.
            val_sum += nums[i];

            // Once the sum is valid, shrink the window from the left
            // to find the smallest possible valid window.
            while(val_sum >= target){
                result = Math.min(result, i + 1 - left);
                val_sum -= nums[left];
                left++;
            }
        }

        // If result was never updated, no valid subarray was found.
        return (result != Integer.MAX_VALUE) ? result : 0;
    }
}
```

---

### 3. Complexity Analysis

- **Time Complexity:** O(n)

  - Although there is a nested `while` loop, each element is visited at most twice. The right pointer `i` visits each element once, and the `left` pointer also visits each element at most once. This gives a total of O(2n) operations, which simplifies to O(n).

- **Space Complexity:** O(1)
  - The algorithm uses only a few variables to store the pointers, sum, and result. The space required is constant and does not depend on the size of the input array.

---

### 4. Key Takeaways & Learnings

- This is a classic dynamic sliding window problem. The key pattern is: expand the window until a condition is met, then shrink the window from the other side as much as possible while the condition remains true.
- Initializing a `min` variable to `Integer.MAX_VALUE` is a very common and useful technique for "find minimum" problems.
- The final check (`result != Integer.MAX_VALUE`) is crucial for handling the edge case where no solution exists.
