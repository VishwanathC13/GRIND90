### [18. 4Sum](https://leetcode.com/problems/4sum/)

**Date:** July 26, 2025
**Pattern:** K-Sum (Two Pointers with Sorting)

---

### 1. Thought Process

- **Inputs & Outputs:** The input is an integer array `nums` and a target integer `target`. The output is a `List<List<Integer>>` containing all _unique_ quadruplets that sum up to the target.

- **Constraints:** The solution set must not contain duplicate quadruplets.

- **My Approach (Generalizing 3Sum):**
  1.  This problem follows the same core pattern as 3Sum. The key is to reduce the k-Sum problem to a (k-1)-Sum problem. Here, we reduce 4Sum to 3Sum.
  2.  First, **sort the array**. This is essential for both the two-pointer approach and for handling duplicates efficiently.
  3.  I'll use nested `for` loops to fix the first two numbers of the quadruplet, `nums[i]` and `nums[j]`.
  4.  For each pair of `(i, j)`, the problem is now reduced to finding two numbers in the rest of the array (from `j+1` to the end) that sum up to `target - nums[i] - nums[j]`. This is a "Two Sum II" problem.
  5.  I'll solve this subproblem with two pointers, `left` starting at `j+1` and `right` starting at the end of the array.
  6.  To prevent integer overflow when calculating the sum, it's safer to use a `long`.
  7.  **Handling Duplicates (The Most Important Part):**
      - **For `nums[i]`:** After the first iteration, check if `nums[i] == nums[i-1]`. If so, `continue` to avoid duplicate quadruplets starting with the same number.
      - **For `nums[j]`:** Similarly, after the first iteration of the inner loop, check if `nums[j] == nums[j-1]`. If so, `continue`.
      - **For `nums[left]` and `nums[right]`:** After finding a valid quadruplet, I must advance the `left` and `right` pointers past any duplicates to ensure the same pair isn't added again. I'll use inner `while` loops for this after adding the result.

---

### 2. Code

```java
class Solution {
    public List<List<Integer>> fourSum(int[] nums, int target) {
        List<List<Integer>> result=new ArrayList<>();
        if (nums==null || nums.length<4) return result;
        Arrays.sort(nums);
        for(int i=0;i<nums.length-3;i++){
            if(i>0 && nums[i]==nums[i-1]) continue;
            for(int j=i+1;j<nums.length-2;j++){
                if(j>i+1 && nums[j]==nums[j-1]) continue;
                int left=j+1;
                int right=nums.length-1;
                while(left<right){
                    long sum=(long)nums[i]+nums[j]+nums[left]+nums[right];
                    if(sum==target){
                        result.add(Arrays.asList(nums[i],nums[j],nums[left],nums[right]));
                        while(left<right && nums[left]==nums[left + 1]) left++;
                        while(left<right && nums[right]==nums[right - 1]) right--;
                        left++;
                        right--;
                    }
                    else  if(sum<target){
                        left++;
                    }
                    else{
                        right--;
                    }
                }
            }
        }
        return result;
    }
}
```

---

### 3. Complexity Analysis

- **Time Complexity:** O(n³)

  - The initial sort takes O(n log n).
  - We have two nested `for` loops, and inside them, a two-pointer `while` loop. This gives a complexity of O(n _ n _ n) = O(n³). This dominates the sorting time.

- **Space Complexity:** O(1)
  - If we exclude the space for the output list, the algorithm uses only a constant amount of extra space for pointers.

---

### 4. Key Takeaways & Learnings

- This problem solidifies the k-Sum pattern: sort the array, use `k-2` loops to fix elements, and then use the two-pointer technique on the remaining part of the array.
- Careful and systematic duplicate handling is critical. It's easy to miss a case, so checking for duplicates at each level (`i`, `j`, `left`, `right`) is essential.
- Always be mindful of potential integer overflows when dealing with sums of multiple numbers, especially in a language like Java. Using a `long` for the sum is a safe and robust practice.
