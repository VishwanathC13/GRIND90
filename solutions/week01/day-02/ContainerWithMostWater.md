### [11. Container With Most Water](https://leetcode.com/problems/container-with-most-water/)

**Date:** July 22, 2025
**Pattern:** Two Pointers (Opposite Ends)

---

### 1. Thought Process

- **Inputs & Outputs:** The input is an integer array `height`. The output is an integer representing the maximum area of water that can be contained between two lines.

- **Constraints:** The input array will have at least two elements.

- **My Approach (Two Pointers):**
  1.  A brute-force approach would be to check every single pair of lines, calculate the area for each, and keep track of the maximum. This would be O(n²), which is likely too slow.
  2.  A more optimal approach uses two pointers, starting from the opposite ends of the array, which represent the widest possible container. Let's call them `a_pointer` (at index 0) and `b_pointer` (at index `height.length - 1`).
  3.  The area of any container is determined by `width * height`. The `width` is `b_pointer - a_pointer`, and the `height` is limited by the shorter of the two lines (`min(height[a_pointer], height[b_pointer])`).
  4.  I'll use a `while` loop that continues as long as `a_pointer < b_pointer`.
  5.  Inside the loop, I'll calculate the current area and update my `max_area` if the current area is larger.
  6.  **The Core Logic (Which pointer to move?):**
      - My goal is to maximize the area. I start with the maximum possible width. To find a potentially larger area, I must increase the height.
      - The height is always limited by the shorter line. If I move the pointer of the _taller_ line inwards, my width will decrease, and my height will either stay the same or get smaller (if the new line is shorter). This can never result in a larger area.
      - Therefore, the only way to potentially find a larger area is to move the pointer of the **shorter** line inwards. By doing this, I sacrifice a little width but gain the _possibility_ of finding a much taller line that could create a larger area.
      - So, if `height[a_pointer] < height[b_pointer]`, I'll move `a_pointer` one step to the right.
      - Otherwise, I'll move `b_pointer` one step to the left.
  7.  The loop will continue until the pointers meet, at which point I will have checked all promising container configurations.

---

### 2. Code

```java
class Solution {
    public int maxArea(int[] height) {
        int max_area = 0;
        int a_pointer = 0;
        int b_pointer = height.length - 1;

        while(a_pointer < b_pointer) {
            // The area is limited by the shorter of the two lines.
            if(height[a_pointer] < height[b_pointer]){
                max_area = Math.max(max_area, height[a_pointer] * (b_pointer - a_pointer));
                // Move the shorter pointer inwards.
                a_pointer += 1;
            } else {
                max_area = Math.max(max_area, height[b_pointer] * (b_pointer - a_pointer));
                // Move the shorter pointer inwards.
                b_pointer -= 1;
            }
        }
        return max_area;
    }
}
```

---

### 3. Complexity Analysis

- **Time Complexity:** O(n)

  - The `a_pointer` and `b_pointer` start at opposite ends and move towards each other. Each pointer will traverse the array at most once. This results in a single pass and a linear time complexity.

- **Space Complexity:** O(1)
  - The algorithm uses only a few variables to store the pointers and the maximum area. The space required does not scale with the size of the input array.

---

### 4. Key Takeaways & Learnings

- This is a classic example of a greedy two-pointer approach. By always making the locally optimal choice (moving the shorter pointer), we arrive at the globally optimal solution.
- The key insight is understanding _why_ we move the shorter pointer. It's because moving the taller pointer can never lead to a better result.
- This pattern is very efficient for problems that involve finding an optimal value between pairs of elements in a sorted or partially-ordered sequence.
