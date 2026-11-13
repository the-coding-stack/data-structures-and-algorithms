# Rotate Image

[Leetcode problem](https://leetcode.com/problems/rotate-image/description/)

## Problem

Given an `n x n` matrix representing an image, rotate it 90 degrees clockwise. Modify the input matrix in place; do not allocate another `n x n` matrix.

### Examples

**Example 1**

```text
Input:  matrix = [[1, 2, 3], [4, 5, 6], [7, 8, 9]]
Output: [[7, 4, 1], [8, 5, 2], [9, 6, 3]]
```

**Example 2**

```text
Input:  matrix = [[5, 1, 9, 11], [2, 4, 8, 10], [13, 3, 6, 7], [15, 14, 12, 16]]
Output: [[15, 13, 2, 5], [14, 3, 4, 1], [12, 6, 8, 9], [16, 7, 10, 11]]
```

### Constraints

- `n == matrix.length == matrix[i].length`
- `1 <= n <= 20`
- `-1000 <= matrix[i][j] <= 1000`

## Solution 1: Use a Second Matrix

For every original element at `(i, j)`, place it at `(j, n - i - 1)` in a new matrix. This mapping moves the first row into the last column, which is the clockwise rotation. Once all elements have been copied, copy the result back into `matrix`.

This solution is straightforward, but it allocates another `n x n` matrix, so it does not meet the problem's in-place requirement.

```java
class Solution {
    public void rotate(int[][] matrix) {
        int n = matrix.length;
        int[][] res = new int[n][n];

        for (int i = 0; i < n; i++) {
            for (int j = 0; j < n; j++) {
                res[j][n - i - 1] = matrix[i][j];
            }
        }

        for (int i = 0; i < n; i++) {
            for (int j = 0; j < n; j++) {
                matrix[i][j] = res[i][j];
            }
        }
    }
}
```

**Complexity**

- **Time:** `O(n^2)`, to fill the result and copy it back.
- **Auxiliary space:** `O(n^2)`, for the result matrix.

## Solution 2: Transpose, Then Reverse Each Row

A clockwise rotation maps the element at `(i, j)` to `(j, n - 1 - i)`. We can do this in place as two simpler operations:

1. **Transpose** the matrix by swapping `matrix[i][j]` with `matrix[j][i]` above the main diagonal. This reflects the matrix across that diagonal.
2. **Reverse every row.** Together, these operations put each value at its clockwise-rotated position.

For example, begin with this matrix:

```text
1  2  3
4  5  6
7  8  9
```

After transposing across the main diagonal:

```text
1  4  7
2  5  8
3  6  9
```

Reverse each row to get the clockwise-rotated matrix:

```text
7  4  1
8  5  2
9  6  3
```

Only visit cells where `j > i` during the transpose so each pair is swapped once and diagonal cells remain unchanged. Row reversal uses swaps from both ends moving toward the center.

```java
class Solution {
    public void rotate(int[][] matrix) {
        int n = matrix.length;

        // Transpose the matrix.
        for (int i = 0; i < n; i++) {
            for (int j = i + 1; j < n; j++) {
                int temp = matrix[i][j];
                matrix[i][j] = matrix[j][i];
                matrix[j][i] = temp;
            }
        }

        // Reverse each row.
        for (int row = 0; row < n; row++) {
            int left = 0;
            int right = n - 1;
            while (left < right) {
                int temp = matrix[row][left];
                matrix[row][left] = matrix[row][right];
                matrix[row][right] = temp;
                left++;
                right--;
            }
        }
    }
}
```

**Complexity**

- **Time:** `O(n^2)`, since each matrix cell is visited a constant number of times.
- **Auxiliary space:** `O(1)`, since the rotation uses only temporary variables and modifies the matrix directly.

