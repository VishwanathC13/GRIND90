### [121. Best Time to Buy and Sell Stock](https://leetcode.com/problems/best-time-to-buy-and-sell-stock/)

**Date:** July 23, 2025
**Pattern:** One-Pass Greedy Approach

---

### 1. Thought Process

- **Inputs & Outputs:** The input is an array of integers `prices`, where `prices[i]` is the price of a stock on day `i`. The output is the maximum profit that can be achieved by buying on one day and selling on a future day. If no profit can be made, the output is 0.

- **Constraints:** I must buy before I sell.

- **My Approach (One-Pass):**
  1.  A brute-force approach would be to check every possible pair of buy and sell days, which would be O(n²). This is too slow.
  2.  A much better approach is to iterate through the `prices` array just once, keeping track of two key pieces of information:
      - `min_val`: The lowest stock price encountered so far. I'll initialize this to the largest possible integer value (`Integer.MAX_VALUE`) to ensure the price on the first day becomes the initial minimum.
      - `max_profit`: The maximum profit found so far. I'll initialize this to 0, as that's the minimum possible profit.
  3.  I will loop through the `prices` array from the first day to the last.
  4.  **Decision Logic for each day's price (`prices[i]`):**
      - First, I'll check if `prices[i]` is a new all-time low. If `prices[i] < min_val`, it means I've found a better day to buy. I'll update `min_val = prices[i]`. I can't sell on this day, so I move on.
      - If `prices[i]` is _not_ a new minimum, it means it's a potential day to sell. I'll calculate the potential profit if I had bought at the `min_val` so far: `prices[i] - min_val`.
      - I'll then compare this potential profit with my current `max_profit`. If it's greater, I've found a new best transaction, so I'll update `max_profit`.
  5.  After the loop finishes, `max_profit` will hold the highest profit found from any valid buy/sell transaction. I can then return this value.

---

### 2. Code

```java
class Solution {
    public int maxProfit(int[] prices) {
     // Track the lowest price seen so far.
     int min_val = Integer.MAX_VALUE;
     // Track the maximum profit found.
     int max_profit = 0;

     for(int i = 0; i < prices.length; i++) {
        // If we find a new minimum price, update our buy point.
        if (prices[i] < min_val) {
            min_val = prices[i];
        }
        // Otherwise, check if selling today gives a better profit.
        else if (prices[i] - min_val > max_profit) {
            max_profit = prices[i] - min_val;
        }
     }

    return max_profit;
    }
}
```

---

### 3. Complexity Analysis

- **Time Complexity:** O(n)

  - The algorithm iterates through the `prices` array exactly once. The time taken is directly proportional to the number of days, `n`.

- **Space Complexity:** O(1)
  - The algorithm uses only two variables (`min_val` and `max_profit`) to store state. The amount of memory used is constant and does not depend on the size of the input array.

---

### 4. Key Takeaways & Learnings

- This problem is a great example of a greedy approach where you don't need to know the future. At any given day `i`, you only need to know the historical minimum price to make the best possible decision for that day.
- It shows that you can often solve problems that seem to require future knowledge by reframing them around the information you have _so far_.
- The logic is simple but powerful: keep track of the lowest valley and calculate the profit from that valley to the current peak.
