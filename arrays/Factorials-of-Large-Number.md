# Factorials of Large Number

[GeeksforGeeks problem](https://www.geeksforgeeks.org/problems/factorials-of-large-numbers2508/1)

## Problem

Given an integer `n`, find `n!` and return its digits as a list in left-to-right order. The result can be much larger than Java's built-in integer types, so it must be calculated and stored one digit at a time.

### Examples

**Example 1**

```text
Input:  n = 5
Output: [1, 2, 0]
Explanation: 5! = 1 * 2 * 3 * 4 * 5 = 120.
```

**Example 2**

```text
Input:  n = 10
Output: [3, 6, 2, 8, 8, 0, 0]
Explanation: 10! = 3628800.
```

**Example 3**

```text
Input:  n = 1
Output: [1]
Explanation: 1! = 1.
```

### Constraints

- `1 <= n <= 10^3`

## Approach: Multiply the Result One Digit at a Time

Store the number's digits in reverse order. For example, represent `120` as `[0, 2, 1]`. This lets us start multiplying at the ones digit. For each multiplier from `2` through `n`, multiply every stored digit and add the carry from the previous digit. Keep the product's ones digit in the list and carry the remaining value to the next position. If any carry remains after processing all digits, append its digits. Reverse the list at the end to return the usual left-to-right order.

### Step-by-Step Example: `5!`

Start with the digits for `1!` in reverse order: `[1]`.

1. Multiply by `2`: `1 * 2 = 2`. Store the ones digit. Digits: `[2]`, which represents `2`.
2. Multiply by `3`: `2 * 3 = 6`. Digits: `[6]`, which represents `6`.
3. Multiply by `4`: `6 * 4 = 24`. Store `4` and carry `2`. Digits: `[4, 2]`, which represents `24`.
4. Multiply by `5`:
   - At the ones position: `4 * 5 + 0 = 20`. Store `0`, carry `2`.
   - At the tens position: `2 * 5 + 2 = 12`. Store `2`, carry `1`.
   - Append the remaining carry `1`.

   The reverse-order digits are now `[0, 2, 1]`, representing `120`. Reverse them to return `[1, 2, 0]`.

```java
import java.util.ArrayList;
import java.util.Collections;

class Solution {
    public ArrayList<Integer> factorial(int n) {
        ArrayList<Integer> digits = new ArrayList<>();
        digits.add(1);

        for (int multiplier = 2; multiplier <= n; multiplier++) {
            int carry = 0;

            for (int i = 0; i < digits.size(); i++) {
                int product = digits.get(i) * multiplier + carry;
                digits.set(i, product % 10);
                carry = product / 10;
            }

            while (carry > 0) {
                digits.add(carry % 10);
                carry /= 10;
            }
        }

        Collections.reverse(digits);
        return digits;
    }
}
```

**Complexity**

- **Time:** `O(n * d)`, where `d` is the number of digits in `n!`; each multiplier processes the current digits.
- **Auxiliary space:** `O(d)` to store the digits of `n!`.
