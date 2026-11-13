# Minimum Moves to Equal Array Elements

[leetcode problem](https://leetcode.com/problems/minimum-moves-to-equal-array-elements/description/)

## Problem

Given an integer array `nums`, return the minimum number of moves required to make all elements equal. In one move, you must increment `n - 1` elements by `1`, where `n` is the array length.

### Examples

**Example 1**

```text
Input:  nums = [1, 2, 3]
Output: 3
Explanation:
[1, 2, 3] -> [2, 3, 3] -> [3, 4, 3] -> [4, 4, 4]
```

**Example 2**

```text
Input:  nums = [1, 1, 1]
Output: 0
```

### Constraints

- `n == nums.length`
- `1 <= nums.length <= 10^5`
- `-10^9 <= nums[i] <= 10^9`
- The answer fits in a 32-bit integer.

## Approach: Find the Minimum and Use the Sum

Simulating the moves is hard because every move touches `n - 1` elements at once. These steps turn that operation into a formula.

**Step 1: incrementing `n - 1` elements is the same as decrementing `1` element.**

All that matters for equality is the gaps between the values, not the values themselves. Adding `1` to every element except `nums[i]` widens the gap between `nums[i]` and everything else by exactly `1`, and so does subtracting `1` from `nums[i]` alone. The two operations differ only by a constant `+1` applied to the whole array, which shifts every value equally and never changes whether the elements are equal.

```text
[1, 2, 3] -> increment n - 1 -> [2, 3, 3]
[1, 2, 3] -> decrement nums[2] -> [1, 2, 2]   (same array, shifted down by 1)
```

So the problem can be restated: with each move subtracting `1` from a single element, how many moves make all elements equal?

**Step 2: expand the total for an arbitrary target `T`.**

Moves only ever decrease values, so every element has to come down to some shared target `T`; no element can rise to meet it. Element `nums[i]` then takes `nums[i] - T` moves, so with `n` as the number of elements the total is:

```text
n = number of elements

moves(T) = (nums[0] - T) + (nums[1] - T) + (nums[2] - T) + ... + (nums[n - 1] - T)

         = nums[0] + nums[1] + nums[2] + ... + nums[n - 1]      <- the values
           - T - T - T - ... - T                                <- T subtracted n times

         = sum(nums) - n * T
```

Each of the `n` brackets contributes one `nums[i]` and one `-T`. Grouping the values gives `sum(nums)`, and grouping the `n` copies of `-T` gives `-n * T`.

**Step 3: choose the largest allowed `T`.**

Because moves cannot raise a value, `T` cannot exceed any element, so `T <= min(nums)`. In `moves(T) = sum(nums) - n * T`, the term `n * T` grows with `T`, so `moves(T)` shrinks as `T` grows. The fewest moves therefore come from the largest allowed target, `T = minimum`:

```text
moves = sum(nums) - n * minimum
```

Checking it on Example 1, where `nums = [1, 2, 3]`: `sum = 6`, `n = 3`, `minimum = 1`, so `moves = 6 - 3 * 1 = 3`, which matches the expected output.

The implementation finds the minimum and sum in one pass. It uses `long` for these intermediate values because the sum and product can exceed the `int` range, even though the problem guarantees that the final answer fits in an `int`.

```java
class Solution {
    public int minMoves(int[] nums) {
        long sum = 0;
        int minimum = nums[0];

        for (int value : nums) {
            sum += value;
            minimum = Math.min(minimum, value);
        }

        long moves = sum - (long) nums.length * minimum;
        return (int) moves;
    }
}
```

**Complexity**

- **Time:** `O(n)`, with one pass through the array.
- **Auxiliary space:** `O(1)`, using a fixed number of variables.
