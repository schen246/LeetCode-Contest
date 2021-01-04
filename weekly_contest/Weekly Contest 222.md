1710. Maximum Units on a Truck
```
You are assigned to put some amount of boxes onto one truck. You are given a 2D array boxTypes, where boxTypes[i] = [numberOfBoxesi, numberOfUnitsPerBoxi]:

numberOfBoxesi is the number of boxes of type i.
numberOfUnitsPerBoxi is the number of units in each box of the type i.
You are also given an integer truckSize, which is the maximum number of boxes that can be put on the truck. You can choose any boxes to put on the truck as long as the number of boxes does not exceed truckSize.

Return the maximum total number of units that can be put on the truck.

Example 1:
Input: boxTypes = [[1,3],[2,2],[3,1]], truckSize = 4
Output: 8
Explanation: There are:
- 1 box of the first type that contains 3 units.
- 2 boxes of the second type that contain 2 units each.
- 3 boxes of the third type that contain 1 unit each.
You can take all the boxes of the first and second types, and one box of the third type.
The total number of units will be = (1 * 3) + (2 * 2) + (1 * 1) = 8.

Example 2:
Input: boxTypes = [[5,10],[2,5],[4,7],[3,9]], truckSize = 10
Output: 91
```
```java
public int maximumUnits(int[][] boxTypes, int truckSize) {
    Arrays.sort(boxTypes, (a, b) -> (b[1] - a[1]));
    int res = 0;
    for (int[] cur : boxTypes) {
        if (truckSize > cur[0]) {
            truckSize -= cur[0];
            res += cur[0] * cur[1];
        } else {
            res += truckSize * cur[1];
            return res;
        }
    }
    return res;
}
```

1711. Count Good Meals
```
A good meal is a meal that contains exactly two different food items with a sum of deliciousness equal to a power of two.

You can pick any two different foods to make a good meal.

Given an array of integers deliciousness where deliciousness[i] is the deliciousness of the i​​​​​​th​​​​​​​​ item of food, return the number of different good meals you can make from this list modulo 109 + 7.

Note that items with different indices are considered different even if they have the same deliciousness value.

Example 1:
Input: deliciousness = [1,3,5,7,9]
Output: 4
Explanation: The good meals are (1,3), (1,7), (3,5) and, (7,9).
Their respective sums are 4, 8, 8, and 16, all of which are powers of 2.

Example 2:
Input: deliciousness = [1,1,1,3,3,3,7]
Output: 15
Explanation: The good meals are (1,1) with 3 ways, (1,3) with 9 ways, and (1,7) with 3 ways.
```
```java
public int countPairs(int[] deliciousness) {
    int res = 0, mod = 1000000007, max = 2 * (1 << 20);
    Map<Integer, Integer> map = new HashMap<>();
    for (int d : deliciousness) {
        for (int num = 1; num <= max; num *= 2) {
            res = (res + map.getOrDefault(num - d, 0)) % mod;
        }
        map.put(d, map.getOrDefault(d, 0) + 1);
    }
    return res;
}
```

1712. Ways to Split Array Into Three Subarrays
```
A split of an integer array is good if:

The array is split into three non-empty contiguous subarrays - named left, mid, right respectively from left to right.
The sum of the elements in left is less than or equal to the sum of the elements in mid, and the sum of the elements in mid is less than or equal to the sum of the elements in right.
Given nums, an array of non-negative integers, return the number of good ways to split nums. As the number may be too large, return it modulo 109 + 7.

Example 1:
Input: nums = [1,1,1]
Output: 1
Explanation: The only good way to split nums is [1] [1] [1].

Example 2:
Input: nums = [1,2,2,2,5,0]
Output: 3
Explanation: There are three good ways of splitting nums:
[1] [2] [2,2,5,0]
[1] [2,2] [2,5,0]
[1,2] [2,2] [5,0]

Example 3:
Input: nums = [3,2,1]
Output: 0
Explanation: There is no good way to split nums.
```
```java
public int waysToSplit(int[] nums) {
    int[] preSum = new int[nums.length];
    preSum[0] = nums[0];
    for (int i = 1; i < nums.length; i++) preSum[i] = nums[i] + preSum[i - 1];
    int sum = preSum[nums.length - 1];
    int res = 0, mod = 1000000007;
    for (int i = 1; i < nums.length - 1; i++) {// i is the right of midsum
        int lr = findRight(preSum, preSum[i], i);// lr the rightmost of midsum's left with lsum[0,lr-1] <= midsum[lr,i] if not return -1
        int ll = findLeft(preSum, preSum[i], sum - preSum[i], i);// ll the leftmost of midsum's left with midsum[ll,i] <= rsum if not return -1
        if (lr == -1 || ll == -1 || ll > lr) continue;
        res += (lr - ll + 1);// ll <= valid <= lr
        res %= mod;
    }
    return res;
}

private int findRight(int[] preSum, int sum, int r) {
    int l = 1, ret = -1;
    while (l <= r) {
        int m = l + (r - l) / 2;
        int lsum = preSum[m - 1];
        int midsum = sum - lsum;
        if (lsum <= midsum) {
            ret = m;
            l = m + 1;
        } else {
            r = m - 1;
        }
    }
    return ret;
}

private int findLeft(int[] preSum, int sum, int rsum, int r) {
    int l = 1, ret = -1;
    while (l <= r) {
        int m = l + (r - l) / 2;
        int midsum = sum - preSum[m - 1];
        if (midsum <= rsum) {
            ret = m;
            r = m - 1;
        } else {
            l = m + 1;
        }
    }
    return ret;
}
```

1713. Minimum Operations to Make a Subsequence
```
You are given an array target that consists of distinct integers and another integer array arr that can have duplicates.

In one operation, you can insert any integer at any position in arr. For example, if arr = [1,4,1,2], you can add 3 in the middle and make it [1,4,3,1,2]. Note that you can insert the integer at the very beginning or end of the array.

Return the minimum number of operations needed to make target a subsequence of arr.

A subsequence of an array is a new array generated from the original array by deleting some elements (possibly none) without changing the remaining elements' relative order. For example, [2,7,4] is a subsequence of [4,2,3,7,2,1,4] (the underlined elements), while [2,4,2] is not.

Example 1:
Input: target = [5,1,3], arr = [9,4,2,3,4]
Output: 2
Explanation: You can add 5 and 1 in such a way that makes arr = [5,9,4,1,2,3,4], then target will be a subsequence of arr.

Example 2:
Input: target = [6,4,8,1,3,2], arr = [4,7,6,2,3,8,6,1]
Output: 3
```
```java
public int minOperations(int[] target, int[] arr) {
    Map<Integer, Integer> map = new HashMap<>();
    for (int i = 0; i < target.length; i++) map.put(target[i], i);
    List<Integer> list = new ArrayList<>();
    for (int a : arr) {
        if (map.containsKey(a)) list.add(map.get(a));
    }
    int[] dp = new int[list.size()];
    int res = 0;
    for (int num : list) {
        int i = Arrays.binarySearch(dp, 0, res, num);
        if (i < 0) i = ~i;
        dp[i] = num;
        if (i == res) res++;
    }
    return target.length - res;
}
```