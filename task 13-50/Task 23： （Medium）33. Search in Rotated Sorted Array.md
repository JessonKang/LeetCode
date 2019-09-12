## Task 23： [（Medium）33 && 81. Search in Rotated Sorted Array](https://leetcode-cn.com/problems/search-in-rotated-sorted-array/)

### 题目 33

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

在数组中查找元素，且要求复杂度不超过O(logn)，故用二分法来找。由于这里是旋转数组，所以在nums[mid]不等于target的情况下，不能简单地根据 target > nums[mid] 而执行 left = mid+1(或target<nums[mid]时执行 right=mid-1)，因为此时mid的右边可能不止有大于nums[mid]的数，可能还有小于nums[mid]的数，需要加其它的判断条件来改变left或right的值。见算法的第3步，算法如下：

1. 设左右边界指针start、end，计算mid = (start+end) / 2；
2. 如果nums[mid] == target，则返回mid，程序结束，否则到3；
3. mid会把nums分成两部分[start...mid] 和 [mid+1...end]（其中一个区间肯定是有序的，可以随便举个例子如：[6,7,1,2,3,4,5]和[3,4,5,6,7,112]）：
   1. 如果[start...mid]部分有序，即nums[start]<=nums[mid]。因为该部分是有序的，所以可以通过比较target和区间端点值的大小关系来判断target是否在有序区域内，即nums[start] <= target < nums[mid]，如果成立，则执行end = mid-1。否则，即target在无序区域内，执行start=mid+1把问题区间转向右边。
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
            mid = start + ((end-start)>>1);//(start+end)/2 的高效写法
            if(nums[mid] == target)
                return mid;
            //前半部分有序的情况           
            if(nums[start] <= nums[mid]){ //注意这里必须带"="，虽然已经规定了数组中不存在重复元素，但是如果数组只有两个元素如[3,1]这种情况时，不带“=”可能就无法找到target=1这个元素。
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

### 题目 81

在上面中给定了target在数组中不存在重复元素，如果存在重复元素时，则上面的程序无法跑通所有的case。

比如：

```java
nums = [1,3,1,1,1]
target = 3

上面的程序返回false.
```

即对于 nums[mid] = nums[left]的情况（也如10111 和 11101），根据上面的程序无法判断是前部分还是后部分有序，此时让left++即可。

**注：这个解法太精辟了！！！**

```java
public boolean search(int[] nums, int target) {
        if (nums == null || nums.length == 0) {
            return false;
        }
        int start = 0;
        int end = nums.length - 1;
        int mid;
        while (start <= end) {
            mid = start + (end - start) / 2;
            if (nums[mid] == target) {
                return true;
            }
            //当无法分清是前半部分还是后半部分有序时，让start++。
            if (nums[start] == nums[mid]) {
                start++;
                continue;
            }
            //前半部分有序
            if (nums[start] < nums[mid]) {
                //target在前半部分
                if (nums[mid] > target && nums[start] <= target) {
                    end = mid - 1;
                } else {  //否则，去后半部分找
                    start = mid + 1;
                }
            } else {
                //后半部分有序
                //target在后半部分
                if (nums[mid] < target && nums[end] >= target) {
                    start = mid + 1;
                } else {  //否则，去后半部分找
                    end = mid - 1;

                }
            }
        }
        //一直没找到，返回false
        return false;

    }

```



### 思考

1. 改变刷题思路，从之前的“龟系”即一道慢慢想、写、琢磨，转变为“兔系”，即尽可能的多做题，掌握思路，把题目多刷几遍。
2. 因为考虑到自己很多题不会做主要原因是掌握的套路太少，所以当前目标是先“兔系”刷到100题再说。

