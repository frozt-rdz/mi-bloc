# Problem
Given the array nums consisting of 2n elements in the form [x1,x2,...,xn,y1,y2,...,yn].

Return the array in the form [x1,y1,x2,y2,...,xn,yn].
## Solution

```
class Solution {
    public int[] shuffle(int[] nums, int n) {
        int[] result = new int[2*n];
        for(int i = 0; i < n; i++) {
            result[2*i] = nums[i];
            result[(2*i) + 1] = nums[i+n];
        }
        return result;
    }
}
```