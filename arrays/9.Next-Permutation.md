# Next Permutation

[Leetcode problem](https://leetcode.com/problems/next-permutation/description/) · [GeeksforGeeks problem](https://www.geeksforgeeks.org/problems/next-permutation5226/1)

## Problem

Given an array of integers `nums`, rearrange it into its next lexicographically greater permutation. If no greater permutation exists, rearrange it into ascending order. Modify the array in place using constant extra space.

### Examples

**Example 1**

```text
Input:  nums = [1, 2, 3]
Output: [1, 3, 2]
Explanation: This is the next lexicographically greater arrangement.
```

**Example 2**

```text
Input:  nums = [3, 2, 1]
Output: [1, 2, 3]
Explanation: The input is the greatest permutation, so wrap around to the smallest.
```

**Example 3**

```text
Input:  nums = [1, 1, 5]
Output: [1, 5, 1]
Explanation: Swapping the last 1 with 5 gives the next arrangement.
```

### Constraints

- `1 <= nums.length <= 100`
- `0 <= nums[i] <= 100`

## Approach: Find the Pivot, Swap, and Reverse

Starting from the right, find the first index `i` where `nums[i] < nums[i + 1]`. This is the pivot: the suffix after it is in descending order, so it cannot be made larger without changing the pivot or something to its left.

If a pivot exists, scan from the right to find the smallest suffix value greater than `nums[i]` (the first value from the right that is greater). Swap those two values. Then reverse the suffix after the pivot. The suffix was descending, and reversing it puts it into ascending order, making the result the smallest permutation that is still greater than the original.

If no pivot exists, the whole array is descending and already the greatest permutation. Reverse the entire array to produce the smallest permutation.

### Step-by-Step Example: `[1, 2, 3]`

1. Scan from the right: `2 < 3`, so the pivot is `2` at index `1`.
2. The rightmost value greater than `2` is `3`. Swap them: `[1, 3, 2]`.
3. Reverse the suffix after the pivot. It contains only `2`, so the result stays `[1, 3, 2]`.

```java
class Solution {
    public void nextPermutation(int[] nums) {
        int n = nums.length;
        int pivot = n - 2;

        while (pivot >= 0 && nums[pivot] >= nums[pivot + 1]) {
            pivot--;
        }

        if (pivot >= 0) {
            int successor = n - 1;
            while (nums[successor] <= nums[pivot]) {
                successor--;
            }
            swap(nums, pivot, successor);
        }

        reverse(nums, pivot + 1, n - 1);
    }

    private void swap(int[] nums, int i, int j) {
        int temp = nums[i];
        nums[i] = nums[j];
        nums[j] = temp;
    }

    private void reverse(int[] nums, int left, int right) {
        while (left < right) {
            swap(nums, left, right);
            left++;
            right--;
        }
    }
}
```

**Complexity**

- **Time:** `O(n)`, with at most two scans and one reversal.
- **Auxiliary space:** `O(1)`, since the array is modified in place.
