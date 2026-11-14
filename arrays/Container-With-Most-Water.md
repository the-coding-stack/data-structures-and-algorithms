# Container With Most Water

[Leetcode problem](https://leetcode.com/problems/container-with-most-water/description/)

## Problem

Given an integer array `height` of length `n`, each value represents a vertical line at that index, from `(i, 0)` to `(i, height[i])`. Choose two lines that, together with the x-axis, form a container that holds the most water. Return the maximum amount of water. The sides must remain vertical; they cannot be slanted.

### Examples

**Example 1**

![Visual explanation for Example 1](https://s3-lc-upload.s3.amazonaws.com/uploads/2018/07/17/question_11.jpg)

```text
Input:  height = [1, 8, 6, 2, 5, 4, 8, 3, 7]
Output: 49
Explanation: The lines at indices 1 and 8 have heights 8 and 7. They form a container with width 7 and height 7, so its area is 7 * 7 = 49.
```

**Example 2**

```text
Input:  height = [1, 1]
Output: 1
Explanation: The width is 1 and the limiting height is 1, so the area is 1.
```

### Constraints

- `n == height.length`
- `2 <= n <= 10^5`
- `0 <= height[i] <= 10^4`

## Approach: Two Pointers

The area formed by lines at `left` and `right` is their distance apart multiplied by the shorter line's height:

```text
(right - left) * min(height[left], height[right])
```

Start with the widest possible container by placing one pointer at each end. After recording its area, move the pointer at the shorter line inward. The shorter line limits the water level. Moving the taller line would make the width smaller while keeping the same limiting line, so it cannot produce a larger area. Moving the shorter line is the only move that might find a taller boundary and improve the area. If both lines have equal height, either pointer can move.

Repeat until the pointers meet, keeping the largest area seen. Each step moves one pointer, so the scan takes linear time and constant extra space.

```java
class Solution {
    public int maxArea(int[] height) {
        int left = 0;
        int right = height.length - 1;
        int maxArea = 0;

        while (left < right) {
            int width = right - left;
            int waterHeight = Math.min(height[left], height[right]);
            maxArea = Math.max(maxArea, width * waterHeight);

            if (height[left] < height[right]) {
                left++;
            } else {
                right--;
            }
        }

        return maxArea;
    }
}
```

**Complexity**

- **Time:** `O(n)`, because each pointer moves inward at most `n - 1` times.
- **Auxiliary space:** `O(1)`, using only a few variables.
