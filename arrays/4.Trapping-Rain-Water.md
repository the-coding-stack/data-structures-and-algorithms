# Trapping Rain Water

[Leetcode problem](https://leetcode.com/problems/trapping-rain-water/description/)

## Problem

Given `n` non-negative integers representing an elevation map, where each bar has width `1`, compute how much water the map can trap after raining.

### Examples

**Example 1**

```text
Input:  height = [0, 1, 0, 2, 1, 0, 1, 3, 2, 1, 2, 1]
Output: 6
```

The bars trap a total of `6` units of water.

**Example 2**

```text
Input:  height = [4, 2, 0, 3, 2, 5]
Output: 9
```

### Constraints

- `n == height.length`
- `1 <= n <= 2 * 10^4`
- `0 <= height[i] <= 10^5`

## Approach: Prefix and Suffix Maximums

Water above an index is limited by the shorter of the tallest bars to its left and right. For each index `i`, its trapped water is therefore:

```text
min(leftMax[i], rightMax[i]) - height[i]
```

Build `leftMax` so each entry stores the tallest bar seen from the left through that index. Build `rightMax` in reverse so each entry stores the tallest bar from that index through the right edge. Then calculate and add the trapped water at every index.

```java
class Solution {
    public int trap(int[] height) {
        int n = height.length;
        int[] leftMax = new int[n];
        int[] rightMax = new int[n];

        leftMax[0] = height[0];
        for (int i = 1; i < n; i++) {
            leftMax[i] = Math.max(leftMax[i - 1], height[i]);
        }

        rightMax[n - 1] = height[n - 1];
        for (int i = n - 2; i >= 0; i--) {
            rightMax[i] = Math.max(rightMax[i + 1], height[i]);
        }

        int water = 0;
        for (int i = 0; i < n; i++) {
            water += Math.min(leftMax[i], rightMax[i]) - height[i];
        }

        return water;
    }
}
```

**Complexity**

- **Time:** `O(n)`, using three passes over the array.
- **Auxiliary space:** `O(n)` for the `leftMax` and `rightMax` arrays.
