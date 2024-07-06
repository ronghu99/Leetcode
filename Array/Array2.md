## [977. Squares of a Sorted Array](https://leetcode.com/problems/squares-of-a-sorted-array/description/)

### 1. Brute force - O(NlogN)
O(N) < O(NlogN): O(N) is better   
Time complexity N + NlogN makes NlogN
```java
class Solution {
    public int[] sortedSquares(int[] nums) {
        // brute force
        int[] res = new int[nums.length];
        for (int i = 0; i < nums.length; i++) {
            res[i] = nums[i] * nums[i];
        }
        // time complexity of Arrays.sort() is NlogN
        Arrays.sort(res);
        return res;
    }
}
```

### 2. Double pointers - O(N)
Start two pointers from left and right end, as it is where max sqaure lies. Then squeeze both pointers to the middle till they meet.
```java
class Solution {
    public int[] sortedSquares(int[] nums) {
        int[] res = new int[nums.length];
        int left = 0;
        int right = nums.length - 1;
        int index = nums.length - 1;
        // time complexity: O(N)
        while (left <= right) {
            int s1 = nums[left] * nums[left];
            int s2 = nums[right] * nums[right];
            if (s1 > s2) {
                res[index] = s1;
                left++;
            } else {
                res[index] = s2;
                right--;
            }
            index--;
        }
        return res;
    }
}
```

## [209. Minimum Size Subarray Sum](https://leetcode.com/problems/minimum-size-subarray-sum/description/)

### 1. Brute force - O(N^2)
```java
class Solution {
    public int minSubArrayLen(int target, int[] nums) {
        int res = Integer.MAX_VALUE;
        // brute force: (N^2)
        for (int i = 0; i < nums.length; i++) {
            int sum = 0;
            for (int j = i; j < nums.length; j++) {
                sum += nums[j];
                if (sum >= target) {
                    res = Math.min(res, j - i + 1);
                    break;
                }
            }
        }
        return res == Integer.MAX_VALUE ? 0: res;
    }
}
```

### 2. Sliding window - O(N)
Important: why we put the for loop for right.
```java
class Solution {
    public int minSubArrayLen(int target, int[] nums) {
        int res = Integer.MAX_VALUE;
        int sum = 0;
        int left = 0;
        for (int right = 0; right < nums.length; right++) {
            sum += nums[right];
            while (sum >= target) {
                res = Math.min(res, right - left + 1);
                sum -= nums[left];
                left += 1;
            }
        }
        return res == Integer.MAX_VALUE ? 0 : res;
    }
}
```

## [59. Spiral Matrix II](https://leetcode.com/problems/spiral-matrix-ii/description/)
Important: how to handle while loop of the boundaries of binary search [left, right] or [left, right)   
This case requires [left, right), each side starting from one corner, but leave the other corner for next side.   
* For each side, start with [ but end the other corder with ), leaving it to the next small for loop
* Be very careful about boundaries of for loops
```java
class Solution {
    public int[][] generateMatrix(int n) {
        int[][] res = new int[n][n];
        int loop = n / 2;
        int offset = 1;
        int startX = 0;
        int startY = 0;
        int figure = 1;
        int i, j;
        for (int k = 0; k < loop; k++) {
            i = startX;
            j = startY;
            // left to right, draw (n - offset) cells
            for (; j < n - offset; j++) {
                res[i][j] = figure++;
            }
            // top to bottom
            for (; i < n - offset; i++) {
                res[i][j] = figure++;
            }
            // right to left
            for (; j > startX; j--) {
                res[i][j] = figure++;
            }
            // bottom to top
            for (; i > startY; i--) {
                res[i][j] = figure++;
            }
            offset++;
            startX++;
            startY++;
        }
        if (n % 2 == 1) {
            res[startX][startY] = figure;
        }
        return res;
    }
}
```
