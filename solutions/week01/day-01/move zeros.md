### [283. Move Zeroes](https://leetcode.com/problems/move-zeroes/)

**Date:** July 21, 2025
**Pattern:** Two Pointers (Slow & Fast) / In-place Manipulation

---

### 1. Thought Process

- **Inputs & Outputs:** The input is an integer array `nums`. The output is the same array, modified in-place, with all zeroes moved to the end while maintaining the relative order of the non-zero elements.

- **Constraints:** The operation must be done in-place, without making a copy of the array.

- **My Approach (Two-pass):**

  1.  The goal is to move all non-zero numbers to the beginning of the array. I can use a separate pointer, `index`, to keep track of the next position where a non-zero element should be placed. `index` will start at 0.
  2.  I'll iterate through the entire array with another pointer, `i`, from beginning to end.
  3.  Inside the loop, if I find a non-zero element at `nums[i]`, I'll place it at the `nums[index]` position and then increment `index`. This effectively overwrites the initial part of the array with all the non-zero elements, in their original order.
  4.  After the first loop finishes, `index` will be pointing to the position right after the last non-zero element.
  5.  Now, I need to fill the rest of the array with zeros. I can run a second loop starting from `index` all the way to the end of the array (`nums.length`).
  6.  In this second loop, I'll set every element `nums[i]` to 0. This fills the remaining spots with the required zeroes.

- **Edge Cases:**
  - An array with no zeroes: The first loop will just copy every element back onto itself. The second loop won't run. Correct.
  - An array with all zeroes: The first loop will do nothing. The second loop will fill the array with zeroes (which it already is). Correct.
  - An empty array: The loops won't run. Correct.

---

### 2. Code

```java
class Solution {
    public void moveZeroes(int[] nums) {
        // 'index' is the slow pointer that marks the position
        // for the next non-zero element.
        int index = 0;

        // First pass: Move all non-zero elements to the front.
        for(int i = 0; i < nums.length ; i++){
            if(nums[i] != 0){
                nums[index++] = nums[i];
            }
        }

        // Second pass: Fill the remaining space with zeroes.
        // 'index' is now at the position where the zeroes should start.
        for(int i = index; i < nums.length ; i++){
            nums[i] = 0;
        }
    }
}
```

---

### 3. Complexity Analysis

- **Time Complexity:** O(n)

  - We iterate through the array `nums` a total of two times in the worst-case scenario (one pass to move non-zeroes, one pass to fill zeroes). This results in a linear time complexity proportional to the size of the array.

- **Space Complexity:** O(1)
  - The algorithm modifies the array in-place. No new arrays or data structures that scale with the input size are created. The pointers (`index`, `i`) use constant extra space.

---

### 4. Key Takeaways & Learnings

- This is a great example of an in-place algorithm that uses a "slow pointer" or "write index" (`index`) to reconstruct the array.
- The two-pass approach is very intuitive and easy to reason about. It cleanly separates the logic for handling non-zero elements and zero elements.
- An optimization could be to perform this in a single pass. In the first loop, when `nums[i]` is not zero, you could swap `nums[i]` with `nums[index]`. This would achieve the same result with slightly fewer total operations, though the overall time complexity remains O(n).
