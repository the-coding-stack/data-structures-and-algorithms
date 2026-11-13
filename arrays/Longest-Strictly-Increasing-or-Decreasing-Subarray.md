# Longest Strictly Increasing or Strictly Decreasing Subarray

[Leetcode problem](https://leetcode.com/problems/longest-strictly-increasing-or-strictly-decreasing-subarray/description/)

## Problem

Given an integer array `nums`, return the length of its longest contiguous subarray that is either strictly increasing or strictly decreasing.

Strictly increasing means every element is greater than the previous one. Strictly decreasing means every element is smaller than the previous one. Equal adjacent values break either kind of run.

### Examples

**Example 1**

```text
Input:  nums = [1, 4, 3, 3, 2]
Output: 2
Explanation: The longest valid subarrays include [1, 4] and [4, 3].
```

**Example 2**

```text
Input:  nums = [3, 3, 3, 3]
Output: 1
Explanation: Equal adjacent values are neither increasing nor decreasing, so each single element is a longest valid subarray.
```

**Example 3**

```text
Input:  nums = [3, 2, 1]
Output: 3
Explanation: The entire array is strictly decreasing.
```

### Constraints

- `1 <= nums.length <= 50`
- `1 <= nums[i] <= 50`

## Approach: Count Increasing and Decreasing Runs

Scan the array once to measure strictly increasing runs, then scan it again to measure strictly decreasing runs.

`inc` tracks the length of the increasing run ending at the current index. If the current value is greater than the previous value, extend the run; otherwise reset its length to `1`. The second scan applies the same logic for decreasing runs. At each step, update `res` with the longest run seen so far.

The answer is at least `1` because the constraints guarantee a non-empty array. The special case for a one-element array returns that answer directly.

```java
class Solution {
    public int longestMonotonicSubarray(int[] nums) {
        int n = nums.length;
        if (n == 1) {
            return 1;
        }

        int inc = 1;
        int dec = 1;
        int result = 1;

        for (int i = 1; i < n; i++) {
            if (nums[i] > nums[i - 1]) {
                inc++;
            } else {
                inc = 1;
            }
            result = Math.max(result, inc);
        }

        for (int i = 1; i < n; i++) {
            if (nums[i] < nums[i - 1]) {
                dec++;
            } else {
                dec = 1;
            }
            result = Math.max(result, dec);
        }

        return result;
    }
}
```

**Complexity**

- **Time:** `O(n)`, from two passes over the array.
- **Auxiliary space:** `O(1)`, because only a fixed number of variables are used.
