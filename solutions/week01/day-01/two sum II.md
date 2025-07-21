### [167. Two Sum II - Input Array Is Sorted](https://leetcode.com/problems/two-sum-ii-input-array-is-sorted/)

**Date:** July 21, 2025
**Pattern:** Two Pointers (Opposite Ends)

---

### 1. Thought Process

- **Inputs & Outputs:** The input is a 1-indexed, sorted integer array `numbers` and an integer `target`. The output is an array of two integers representing the 1-based indices of the two numbers that add up to the target.

- **Constraints:** The array is already sorted in non-decreasing order. There is exactly one solution. I cannot use the same element twice.

- **My Approach (Two Pointers):**
  1.  Since the array is sorted, this is a perfect use case for the two-pointer pattern, starting from opposite ends.
  2.  Initialize a left pointer, `a_pointer`, at the beginning of the array (index 0).
  3.  Initialize a right pointer, `b_pointer`, at the end of the array (index `numbers.length - 1`).
  4.  Use a `while` loop that continues as long as the left pointer is less than or equal to the right pointer.
  5.  Inside the loop, calculate the `sum` of the values at the two pointers: `numbers[a_pointer] + numbers[b_pointer]`.
  6.  **Decision Logic:**
      - If `sum > target`, it means our sum is too large. Since the array is sorted, I need to decrease the sum. I can do this by moving the right pointer, `b_pointer`, one step to the left (`b_pointer--`).
      - If `sum < target`, our sum is too small. I need to increase the sum, which I can do by moving the left pointer, `a_pointer`, one step to the right (`a_pointer++`).
      - If `sum == target`, I have found the solution. The problem asks for 1-based indices, so I will return a new array with `{a_pointer + 1, b_pointer + 1}`.
  7.  The problem guarantees a solution exists, so the loop will always find the answer and return from within the `else` block. The final return statement is a fallback.

---

### 2. Code

```java
class Solution {
    public int[] twoSum(int[] numbers, int target) {
        int a_pointer = 0;
        int b_pointer = numbers.length - 1;

        while(a_pointer <= b_pointer){
            int sum = numbers[a_pointer] + numbers[b_pointer];

            if (sum > target){
                b_pointer -= 1;
            }
            else if (sum < target){
                a_pointer += 1;
            }
            else {
                // Found the solution, return 1-based indices.
                return new int []{a_pointer + 1, b_pointer + 1};
            }
        }
        // Fallback return, though the problem guarantees a solution is found in the loop.
        return new int[] {a_pointer + 1, b_pointer + 1};
    }
}
```

---

### 3. Complexity Analysis

- **Time Complexity:** O(n)

  - In each iteration of the `while` loop, we move one of the pointers closer to the other. In the worst-case scenario, the pointers will traverse the entire array once, resulting in a linear time complexity.

- **Space Complexity:** O(1)
  - The algorithm uses only a few variables (`a_pointer`, `b_pointer`, `sum`) to store data, regardless of the input array's size. This is a constant space solution.

---

### 4. Key Takeaways & Learnings

- This problem highlights the power of the two-pointer technique when an array is **sorted**. The sorted property is what allows us to make intelligent decisions about which pointer to move.
- This is a significant improvement over the classic "Two Sum" problem, where a hash map (O(n) space) is required because the array is unsorted.
- The logic of moving pointers inwards based on comparing the current sum to the target is a fundamental pattern that appears in many other problems.
