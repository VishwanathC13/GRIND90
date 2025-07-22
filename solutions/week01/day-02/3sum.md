### [15. 3Sum](https://leetcode.com/problems/3sum/)

**Date:** July 22, 2025
**Pattern:** Two Pointers with Sorting

---

### 1. Thought Process

- **Inputs & Outputs:** The input is an integer array `nums`. The output is a `List<List<Integer>>` containing all _unique_ triplets `[nums[i], nums[j], nums[k]]` such that their sum is zero.

- **Constraints:** The solution set must not contain duplicate triplets.

- **My Approach (Sort + Two Pointers):**
  1.  A brute-force approach of checking every possible triplet would be O(n³), which is too slow for typical constraints. The key to optimization is **sorting the array first**.
  2.  After sorting, I can iterate through the array with a single `for` loop, using the pointer `i` to fix the first element of a potential triplet.
  3.  For each `nums[i]`, the problem is reduced to finding two other numbers in the rest of the array (from index `i+1` to the end) that sum up to `-nums[i]`. This is now a "Two Sum II" problem on a subarray.
  4.  I'll use the two-pointer technique for this subproblem. Initialize `low = i + 1` and `high = nums.length - 1`.
  5.  Inside a `while (low < high)` loop, I'll check the sum of `nums[low] + nums[high]`:
      - If the sum is greater than the target (`-nums[i]`), I need a smaller sum, so I'll decrement `high`.
      - If the sum is less than the target, I need a larger sum, so I'll increment `low`.
      - If the sum is exactly equal to the target, I've found a valid triplet. I'll add `[nums[i], nums[low], nums[high]]` to my output list.
  6.  **Handling Duplicates (The Crucial Part):**
      - **For the first element (`nums[i]`):** If `i` is not the first element, I must check if it's the same as the previous element (`nums[i] == nums[i-1]`). If it is, I should `continue` to the next iteration to avoid processing the same starting number and generating duplicate triplets. This is handled by the `if(i==0 || (i>0 && nums[i]!=nums[i-1]))` condition.
      - **After finding a valid triplet:** Once I find a match with `nums[low]` and `nums[high]`, it's possible the next elements are duplicates (e.g., `[-2, -1, -1, 0, 1, 1, 3]`). To avoid adding `[-2, -1, 3]` twice, I must advance my `low` and `high` pointers past all their respective duplicates. I'll use inner `while` loops for this before doing the final `low++` and `high--`.

---

### 2. Code

```java
class Solution {
    public List<List<Integer>> threeSum(int[] nums) {
        // Sort the array to enable the two-pointer approach.
        Arrays.sort(nums);
        List<List<Integer>> output_arr = new LinkedList<>();

        // Iterate through the array to fix the first element of the triplet.
        for (int i = 0; i < nums.length - 2; i++) {
            // Skip duplicate values for the first element.
            if (i == 0 || (i > 0 && nums[i] != nums[i - 1])) {
                int low = i + 1;
                int high = nums.length - 1;
                int sum = 0 - nums[i]; // The target for the other two elements.

                while (low < high) {
                    if (nums[low] + nums[high] == sum) {
                        output_arr.add(Arrays.asList(nums[i], nums[low], nums[high]));

                        // Skip duplicates for the second and third elements.
                        while (low < high && nums[low] == nums[low + 1]) low++;
                        while (low < high && nums[high] == nums[high - 1]) high--;

                        low++;
                        high--;
                    } else if (nums[low] + nums[high] > sum) {
                        high--;
                    } else {
                        low++;
                    }
                }
            }
        }
        return output_arr;
    }
}
```

---

### 3. Complexity Analysis

- **Time Complexity:** O(n²)

  - The initial sort of the array takes O(n log n).
  - The main loop runs `n` times. Inside it, the two-pointer `while` loop runs at most `n` times. This nested loop structure results in an O(n²) complexity.
  - The total complexity is O(n log n + n²), which is dominated by O(n²).

- **Space Complexity:** O(1) or O(n)
  - The space complexity of the sorting algorithm in Java is O(log n) for primitives.
  - If we don't count the output list (`output_arr`), the space used for pointers is O(1).
  - If we must count the output list, the space could be up to O(n) in some cases. Generally, for complexity analysis, the space for the return value is excluded.

---

### 4. Key Takeaways & Learnings

- This problem is a masterclass in combining sorting with the two-pointer pattern.
- The most difficult part is correctly identifying and handling all three potential sources of duplicates to ensure the final output is unique.
- The pattern of "reduce a k-sum problem to a (k-1)-sum problem" is very powerful. Here, we reduced 3Sum to a series of 2Sum-II problems.
