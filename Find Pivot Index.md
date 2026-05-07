#  Problem
Given an array of integers nums, calculate the pivot index of this array.

The pivot index is the index where the sum of all the numbers strictly to the left of the index is equal to the sum of all the numbers strictly to the index's right.

If the index is on the left edge of the array, then the left sum is 0 because there are no elements to the left. This also applies to the right edge of the array.

Return the leftmost pivot index. If no such index exists, return -1.
##  Solution
```
class Solution {
    public int pivotIndex(int[] nums) {
        int index = -1;

        int sumder = 0, sumizq = 0;

        for (int i = 0; i < nums.length; i++) {
            sumizq = 0;
            sumder = 0;
            if (i == 0) {
                sumizq = 0;
            } else {
                for (int k = i - 1; k >= 0; k--) {
                    sumizq += nums[k];
                }
            }
            for (int j = i + 1; j < nums.length; j++) {
                sumder += nums[j];
            }
            if (sumizq == sumder) {
                index = i;
                return index;
            }
        }

        return index;
    }
}
```
