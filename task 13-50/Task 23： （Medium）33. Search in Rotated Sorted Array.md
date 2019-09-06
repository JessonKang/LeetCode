## Task 23： [（Medium）33. Search in Rotated Sorted Array](https://leetcode-cn.com/problems/search-in-rotated-sorted-array/)

### 题目

Suppose an array sorted in ascending order is rotated at some pivot unknown to you beforehand.

(i.e., [0,1,2,4,5,6,7] might become [4,5,6,7,0,1,2]).

You are given a target value to search. If found in the array return its index, otherwise return -1.

You may assume no duplicate exists in the array.

Your algorithm's runtime complexity must be in the order of O(log n).

**Example 1:**

> Input: nums = [4,5,6,7,0,1,2], target = 0
> Output: 4

**Example 2:**

> Input: nums = [4,5,6,7,0,1,2], target = 3
> Output: -1

### 思路

在数组中查找元素，且要求复杂度不超过O(logn)，故用二分法来找。由于这里是旋转数组，所以要考虑到一些其它的情况，算法如下：

1. 设左右边界指针start、end，计算mid = (start+end) / 2；
2. 如果nums[mid] == target，则返回mid，程序结束，否则到3；
3. mid会把nums分成两部分[start...mid] 和 [mid+1...end]：
   1. 如果[start...mid]部分有序，即nums[start]<=nums[mid]。判断target是否在有序区域内，即nums[start] <= target < nums[mid]，如果满足，即执行end = mid-1。否则，即target在无序区域内，执行start=mid+1把问题区间转向右边。
   2. 如果是[mid...end]有序的情况，则看target是否落在有序区域，即nums[mid] < target <= nums[end]，如果满足，则start = mid+1，否则，即target在无序区域，执行end=mid-1.

```java
class Solution {
    public int search(int[] nums, int target) {
        if(nums.length==0)
            return -1;
        int start = 0;
        int end = nums.length -1;
        int mid;
        while(start <= end){
            mid = start + ((end-start)>>1);
            if(nums[mid] == target)
                return mid;
            //前半部分有序的情况           
            if(nums[start] <= nums[mid]){
                //target在有序区间里
                if(target<nums[mid] && target>=nums[start])
                    end = mid-1;
                else//target在无序区间里
                    start = mid+1;
            }else{ //后半部分有序
                //target在有序区间里
                if(target>nums[mid] && target<=nums[end])
                    start = mid+1;
                else //target在无序区间里
                    end = mid-1;
            }
      }
        return -1;
    }
}
```

### 思考

1. 改变刷题思路，从之前的“龟系”即一道慢慢想、写、琢磨，转变为“兔系”，即尽可能的多做题，掌握思路，把题目多刷几遍。
2. 因为考虑到自己很多题不会做主要原因是掌握的套路太少，所以当前目标是先“兔系”刷到100题再说。