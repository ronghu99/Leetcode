
## Array
Container object that holds a fixed number of values of a single type. Array stored in consecutive memory locatios. 

## [704 Binary Search](https://leetcode.com/problems/binary-search/description/)
### 1. [left, right]
left == right is meaningful in [left, right]
```java
class Solution {
    public int search(int[] nums, int target) {
        int left = 0;
        int right = nums.length - 1;
        while (left <= right) {
            // + operator has higher priority than >>
            int mid = left + ((right - left) >> 1);
            if (nums[mid] == target) {
                return mid;
            }
            if (nums[mid] > target) {
                right = mid - 1;
            } else {
                left = mid + 1;
            }
        }
        return -1;
    }
}
```

### 2. [left, right)
left == right is meaningless in [left, right)
```java
class Solution {
    public int search(int[] nums, int target) {
        int left = 0;
        int right = nums.length;
        while (left < right) {
            int mid = left + ((right - left) >> 1);
            if (nums[mid] == target) {
                return mid;
            }
            if (nums[mid] > target) {
                right = mid;
            } else {
                left = mid + 1;
            }
        }
        return -1;
    }
}
```


## [27 Remove Element](https://leetcode.com/problems/remove-element/description/)
Space require in-place.
### 1. Brute force - O(N^2)
```java
class Solution {
    public int removeElement(int[] nums, int val) {
        int k = nums.length;
        for (int i = 0; i < k; i++) {
            if (nums[i] == val) {
                for (int j = i + 1; j < k; j++) {
                    nums[j - 1] = nums[j];
                }
                i -= 1;
                k -= 1;
            }
        }
        return k;
    }
}
```

### 2. Double pointers - O(N)
Keep fast pointer moving, when valid, assign and move slow. Otherwise, fast pointer skip.
```java
class Solution {
    public int removeElement(int[] nums, int val) {
        int slow = 0;
        for (int fast = 0; fast < nums.length; fast++) {
            if (nums[fast] != val) {
                nums[slow] = nums[fast];
                slow += 1;
            }
        }
        return slow;
    }
}
```