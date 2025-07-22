### [75. Sort Colors](https://leetcode.com/problems/sort-colors/)

**Date:** July 22, 2025
**Pattern:** Three Pointers (Dutch National Flag Algorithm)

---

### 1. Thought Process

- **Inputs & Outputs:** The input is an integer array `nums` containing only the numbers 0, 1, and 2. The output is the same array, sorted in-place.

- **Constraints:** I must solve this without using the library's sort function.

- **My Approach (Dutch National Flag Algorithm):**
  1.  A simple approach would be to count the occurrences of 0s, 1s, and 2s and then overwrite the original array. However, this requires two passes. A more efficient, one-pass solution is the Dutch National Flag algorithm.
  2.  This algorithm uses three pointers:
      - `start`: Points to the position where the next 0 should go. It starts at index 0.
      - `end`: Points to the position where the next 2 should go. It starts at the last index.
      - `index`: The current element being considered. It starts at index 0 and moves through the array.
  3.  The array is conceptually divided into four sections: 0s are to the left of `start`, 1s are between `start` and `index`, unknown elements are between `index` and `end`, and 2s are to the right of `end`.
  4.  I'll iterate with a `while` loop as long as `index <= end`.
  5.  **Decision Logic based on `nums[index]`:**
      - If `nums[index] == 0`: The 0 is in the wrong place. I need to swap it with the element at the `start` pointer. After swapping, I know the element at `start` is now correct, and the element I just moved to `index` (from `start`) has been processed, so I can increment both `start` and `index`.
      - If `nums[index] == 2`: The 2 is in the wrong place. I need to swap it with the element at the `end` pointer. After swapping, I know the element at `end` is now correct, so I decrement `end`. **Crucially, I do not increment `index`**, because the new element at `nums[index]` (which came from `end`) has not been processed yet and needs to be checked in the next iteration.
      - If `nums[index] == 1`: The 1 is in the correct section (between `start` and `index`). I don't need to do anything but move on to the next element, so I just increment `index`.
  6.  The loop terminates when `index` crosses `end`, at which point the array will be fully sorted.

---

### 2. Code

```java
class Solution {
    public void sortColors(int[] nums) {
        if(nums.length == 0 || nums.length == 1) return;

        int start = 0;
        int end = nums.length - 1;
        int index = 0;

        // Loop until the current index passes the end pointer.
        while (index <= end && start < end) {
            if (nums[index] == 0) {
                // If it's a 0, swap with the start pointer's element.
                nums[index] = nums[start];
                nums[start] = 0;
                start++;
                index++;
            } else if (nums[index] == 2) {
                // If it's a 2, swap with the end pointer's element.
                nums[index] = nums[end];
                nums[end] = 2;
                end--;
                // Don't increment index, as the new element at index needs to be processed.
            } else {
                // If it's a 1, it's in the right place, just move to the next element.
                index++;
            }
        }
    }
}
```

---

### 3. Complexity Analysis

- **Time Complexity:** O(n)

  - Although there are three pointers, the `index` pointer traverses the array only once from beginning to end. Each element is visited at most a constant number of times, resulting in a single-pass, linear time complexity.

- **Space Complexity:** O(1)
  - The sorting is done entirely in-place. The three pointers use constant extra space, regardless of the input array's size.

---

### 4. Key Takeaways & Learnings

- The Dutch National Flag algorithm is a highly efficient and classic solution for the 3-way partitioning problem.
- The key to getting it right is understanding the pointer movements, especially why `index` is _not_ incremented when a 2 is swapped.
- This pattern is a more advanced form of the two-pointer technique and is very useful for partitioning an array into multiple groups in a single pass.
