### [125. Valid Palindrome](https://leetcode.com/problems/valid-palindrome/)

**Date:** July 21, 2025
**Pattern:** Two Pointers on a Pre-processed String

---

### 1. Thought Process

- **Inputs & Outputs:** The input is a string `s`. The output is a boolean, indicating whether it's a palindrome or not.

- **Constraints:** The problem requires ignoring case and non-alphanumeric characters.

- **My Approach (Brute-force/Two-pass):**
  1. First, create a new, clean string to work with. I'll call it `fixed_string`.
  2. Iterate through the original input string `s` character by character.
  3. For each character, check if it's a letter or a digit. If it is, append it to `fixed_string`. This effectively filters out all punctuation and spaces.
  4. Once the loop is done, convert the entire `fixed_string` to lowercase to handle the case-insensitivity requirement.
  5. Now, use the Two Pointers pattern on the `fixed_string`. Initialize `a_pointer` at the start (index 0) and `b_pointer` at the end.
  6. In a `while` loop, compare the characters at `a_pointer` and `b_pointer`. If they ever don't match, I know it's not a palindrome, so I can return `false` immediately.
  7. If they do match, move both pointers inwards (`a_pointer++`, `b_pointer--`).
  8. If the loop completes without finding any mismatches, the string must be a palindrome, so I can return `true`.

---

### 2. Code

```java
class Solution {
    public boolean isPalindrome(String s) {

        String fixed_string = "";

        for(char c : s.toCharArray()){
            if(Character.isDigit(c) || Character.isLetter(c)){
                fixed_string += c;
            }
        }

        fixed_string = fixed_string.toLowerCase();

        int a_pointer = 0;
        int b_pointer = fixed_string.length()-1;

        while(a_pointer <= b_pointer){
            if(fixed_string.charAt(a_pointer) != fixed_string.charAt(b_pointer)){
                return false;
            }
            a_pointer += 1;
            b_pointer -= 1;
        }
        return true;
    }
}
```

---

### 3. Complexity Analysis

- **Time Complexity:** O(n)

  - The first loop to build `fixed_string` iterates through all `n` characters of the input string. The second `while` loop iterates through the `m` characters of the `fixed_string`. In the worst case, `m` is equal to `n`. Therefore, the total time is O(n + n) = O(n).

- **Space Complexity:** O(n)
  - I create a new string, `fixed_string`. In the worst-case scenario (if the input string has no special characters), `fixed_string` will have the same length as the input string `s`. This requires O(n) extra space.

---

### 4. Key Takeaways & Learnings

- This two-pass approach is very clear and works perfectly. It separates the "cleaning" logic from the "checking" logic.
- The main learning point is the space complexity. While this solution is correct, it's not the most space-optimal.
- A more optimal way to solve this would be to use the Two Pointers on the _original_ string and perform the character validation and comparison inside the loop. This would achieve an O(1) space complexity, which is the next level of optimization to aim for.
