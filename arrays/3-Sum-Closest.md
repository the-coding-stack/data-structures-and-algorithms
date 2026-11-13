# 3Sum Closest

[Leetcode problem](https://leetcode.com/problems/3sum-closest/description/)

## Problem

Given an integer array `nums` and an integer `target`, choose three values at distinct indices so their sum is as close as possible to `target`. Return that sum. The problem guarantees exactly one closest sum.

### Examples

**Example 1**

```text
Input:  nums = [-1, 2, 1, -4], target = 1
Output: 2
Explanation: -1 + 2 + 1 = 2, which is closest to 1.
```

**Example 2**

```text
Input:  nums = [0, 0, 0], target = 1
Output: 0
Explanation: The only possible sum is 0.
```

### Constraints

- `3 <= nums.length <= 500`
- `-1000 <= nums[i] <= 1000`
- `-10^4 <= target <= 10^4`

## Approach 1: Brute Force

Try every combination of three distinct indices with three nested loops. Keep the sum whose absolute difference from `target` is smallest. Since the constraints allow at most 500 values, this approach is simple but can take cubic time.

```java
class Solution {
    public int threeSumClosest(int[] nums, int target) {
        int closestSum = nums[0] + nums[1] + nums[2];

        for (int i = 0; i < nums.length - 2; i++) {
            for (int j = i + 1; j < nums.length - 1; j++) {
                for (int k = j + 1; k < nums.length; k++) {
                    int sum = nums[i] + nums[j] + nums[k];

                    if (Math.abs(target - sum) < Math.abs(target - closestSum)) {
                        closestSum = sum;
                    }
                }
            }
        }

        return closestSum;
    }
}
```

**Complexity**

- **Time:** `O(n^3)`, checking every triple of indices.
- **Auxiliary space:** `O(1)`.

## Approach 2: Sorting and Two Pointers

Sort the array, then fix each value in turn as the first value of a triple. Set one pointer to the next index and another to the end of the array. For each pair of pointers, update the closest sum seen so far. If the sum is less than `target`, move the left pointer forward to increase it; otherwise, move the right pointer backward to decrease it. If a sum equals the target, return immediately because no sum can be closer.

```java
import java.util.Arrays;

class Solution {
    public int threeSumClosest(int[] nums, int target) {
        Arrays.sort(nums);
        int closestSum = nums[0] + nums[1] + nums[2];

        for (int i = 0; i < nums.length - 2; i++) {
            int left = i + 1;
            int right = nums.length - 1;

            while (left < right) {
                int sum = nums[i] + nums[left] + nums[right];

                if (Math.abs(target - sum) < Math.abs(target - closestSum)) {
                    closestSum = sum;
                }

                if (sum < target) {
                    left++;
                } else if (sum > target) {
                    right--;
                } else {
                    return sum;
                }
            }
        }

        return closestSum;
    }
}
```

**Complexity**

- **Time:** `O(n^2)`, after sorting, each fixed index scans the remaining values with two pointers.
- **Auxiliary space:** `O(log n)` for the sorting implementation's stack; the two-pointer scan uses `O(1)` extra space.
