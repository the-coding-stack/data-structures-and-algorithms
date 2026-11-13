# Two Sum II: Input Array Is Sorted

[Leetcode problem](https://leetcode.com/problems/two-sum-ii-input-array-is-sorted/description/)

## Problem

Given a **1-indexed**, non-decreasing sorted integer array `numbers`, find two different elements that add up to `target`. Return their 1-based indices as `[index1, index2]`, with `index1 < index2`.

Exactly one solution exists. The same element cannot be used twice, and the solution must use constant extra space.

### Examples

**Example 1**

```text
Input:  numbers = [2, 7, 11, 15], target = 9
Output: [1, 2]
Explanation: 2 + 7 = 9.
```

**Example 2**

```text
Input:  numbers = [2, 3, 4], target = 6
Output: [1, 3]
Explanation: 2 + 4 = 6.
```

**Example 3**

```text
Input:  numbers = [-1, 0], target = -1
Output: [1, 2]
Explanation: -1 + 0 = -1.
```

### Constraints

- `2 <= numbers.length <= 3 * 10^4`
- `-1000 <= numbers[i] <= 1000`
- `numbers` is sorted in non-decreasing order.
- `-1000 <= target <= 1000`

## Approach: Two Pointers

Start one pointer at the beginning and one at the end. If their sum is too large, move the right pointer left to reduce it. If the sum is too small, move the left pointer right to increase it. When the sum matches the target, return the pointers converted to 1-based indices.

```java
class Solution {
    public int[] twoSum(int[] numbers, int target) {
        int left = 0;
        int right = numbers.length - 1;

        while (left < right) {
            int sum = numbers[left] + numbers[right];

            if (sum < target) {
                left++;
            } else if (sum > target) {
                right--;
            } else {
                return new int[] {left + 1, right + 1};
            }
        }

        // The problem guarantees a solution, so this line is unreachable.
        return new int[0];
    }
}
```

**Complexity**

- **Time:** `O(n)`, because each pointer moves across the array at most once.
- **Auxiliary space:** `O(1)`, excluding the constant-size returned index array.
