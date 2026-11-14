# Spiral Matrix

[Leetcode problem](https://leetcode.com/problems/spiral-matrix/description/)

## Problem

Given an `m x n` matrix, return all elements of the matrix in spiral order.

### Examples

**Example 1**

![Visual explanation for Example 1](https://assets.leetcode.com/uploads/2020/11/13/spiral1.jpg)

```text
Input:  matrix = [[1, 2, 3], [4, 5, 6], [7, 8, 9]]
Output: [1, 2, 3, 6, 9, 8, 7, 4, 5]
```

**Example 2**

![Visual explanation for Example 2](https://assets.leetcode.com/uploads/2020/11/13/spiral.jpg)

```text
Input:  matrix = [[1, 2, 3, 4], [5, 6, 7, 8], [9, 10, 11, 12]]
Output: [1, 2, 3, 4, 8, 12, 11, 10, 9, 5, 6, 7]
```

### Constraints

- `m == matrix.length`
- `n == matrix[i].length`
- `1 <= m, n <= 10`
- `-100 <= matrix[i][j] <= 100`

## Approach: Shrink the Boundaries

Keep four boundaries around the unvisited part of the matrix: `top`, `bottom`, `left`, and `right`. On each round, traverse the top edge from left to right, then the right edge from top to bottom, the bottom edge from right to left, and the left edge from bottom to top. After traversing an edge, move its boundary inward.

Continue while both dimensions still have an unvisited row and column (`top <= bottom` and `left <= right`). After finishing the top edge, check the row bounds before traversing the bottom edge; after finishing the right edge, check the column bounds before traversing the left edge. These checks avoid adding elements twice when the remaining layer is a single row or column.

```java
import java.util.ArrayList;
import java.util.List;

class Solution {
    public List<Integer> spiralOrder(int[][] matrix) {
        int rows = matrix.length;
        int cols = matrix[0].length;
        int top = 0;
        int bottom = rows - 1;
        int left = 0;
        int right = cols - 1;
        List<Integer> result = new ArrayList<>();

        while (top <= bottom && left <= right) {
            // Traverse the top edge from left to right.
            for (int col = left; col <= right; col++) {
                result.add(matrix[top][col]);
            }
            top++;

            // Traverse the right edge from top to bottom.
            for (int row = top; row <= bottom; row++) {
                result.add(matrix[row][right]);
            }
            right--;

            // The remaining row may already have been visited.
            if (top <= bottom) {
                for (int col = right; col >= left; col--) {
                    result.add(matrix[bottom][col]);
                }
                bottom--;
            }

            // The remaining column may already have been visited.
            if (left <= right) {
                for (int row = bottom; row >= top; row--) {
                    result.add(matrix[row][left]);
                }
                left++;
            }
        }

        return result;
    }
}
```

**Complexity**

- **Time:** `O(m * n)`, because every matrix element is added once.
- **Auxiliary space:** `O(1)` excluding the returned list; the result itself uses `O(m * n)` space.
