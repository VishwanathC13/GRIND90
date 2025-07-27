### [904. Fruit Into Baskets](https://leetcode.com/problems/fruit-into-baskets/)

**Date:** July 27, 2025
**Pattern:** Sliding Window (Space-Optimized)

---

### 1. Thought Process

- **Inputs & Outputs:** The input is an integer array `fruits`. The output is an integer representing the maximum number of fruits that can be collected, which translates to finding the "longest subarray with at most two distinct elements."

- **Constraints:** The array can be large, so an efficient solution is needed.

- **My Approach (Space-Optimized Sliding Window):**
  1.  A standard sliding window approach would use a hash map to keep track of the fruit types in the current window. This works but uses O(k) space (where k is the number of distinct fruits, in this case, 2).
  2.  This solution is a more optimized approach that uses only constant extra space (O(1)) by tracking the state with a few variables instead of a hash map.
  3.  **State Variables:**
      - `max`: The overall maximum length found so far.
      - `current_max`: The length of the current valid window (with at most two fruit types).
      - `last_fruit`: The type of the most recently seen fruit.
      - `second_last_fruit`: The type of the fruit seen before the `last_fruit`.
      - `last_fruit_count`: The count of the current contiguous block of `last_fruit`.
  4.  I'll iterate through the `fruits` array one by one.
  5.  **Decision Logic for each `fruit`:**
      - **If the `fruit` is one of the two types we are currently tracking (`last_fruit` or `second_last_fruit`):** We are in a valid window, so I can simply increment `current_max`.
      - **If the `fruit` is a new, third type:** The window is now invalid. The new valid window must start from the beginning of the most recent contiguous block of fruit. For example, if we have `[1, 2, 2, 2, 3]`, when we see `3`, the new window becomes `[2, 2, 2, 3]`. The length of this new window is the count of the last fruit (`last_fruit_count`) plus the new fruit (1). So, `current_max` is reset to `last_fruit_count + 1`.
      - **Updating Counts and Fruit Types:**
        - I need to continuously track the length of the current block of identical fruit. If the current `fruit` is the same as `last_fruit`, I increment `last_fruit_count`. If it's different, I reset `last_fruit_count` to 1.
        - When a new fruit type is encountered (i.e., `fruit != last_fruit`), I need to update my pointers: the old `last_fruit` becomes the `second_last_fruit`, and the current `fruit` becomes the new `last_fruit`.
  6.  In every iteration, I update the overall `max` with the `current_max` if it's larger.

---

### 2. Code

```java
class Solution {
    public int totalFruit(int[] fruits) {
        int last_fruit = -1;
        int second_last_fruit = -1;
        int last_fruit_count = 0;
        int current_max = 0;
        int max = 0;

        for (Integer fruit : fruits) {
            // If the fruit is one of the two types we are tracking,
            // we can extend the current window.
            if (fruit == last_fruit || fruit == second_last_fruit) {
                current_max += 1;
            } else {
                // If it's a new fruit, the new window is the last block + this one.
                current_max = last_fruit_count + 1;
            }

            // Update the count of the contiguous block of the last fruit.
            if (fruit == last_fruit) {
                last_fruit_count += 1;
            } else {
                last_fruit_count = 1;
            }

            // Update the last and second-to-last fruit types if we see a new type.
            if (fruit != last_fruit) {
                second_last_fruit = last_fruit;
                last_fruit = fruit;
            }

            // Update the overall max.
            max = Math.max(current_max, max);
        }
        return max;
    }
}
```

---

### 3. Complexity Analysis

- **Time Complexity:** O(n)

  - The algorithm iterates through the `fruits` array exactly once, performing O(1) work at each step.

- **Space Complexity:** O(1)
  - This is the key advantage of this approach. It uses only a fixed number of integer variables, resulting in constant space complexity, which is better than a hash map-based solution.

---

### 4. Key Takeaways & Learnings

- This problem, which seems complex, is a variation of "longest subarray with at most K distinct elements" where K=2.
- While a hash map is a general solution for this pattern, this problem can be solved with a more space-efficient approach by carefully tracking the state of the last two distinct elements seen.
- The logic for resetting `current_max` to `last_fruit_count + 1` is the core insight for the space-optimized version.
