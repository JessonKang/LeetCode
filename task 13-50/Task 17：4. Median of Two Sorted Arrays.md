## Task 17：[4.（hard） Median of Two Sorted Arrays](https://leetcode-cn.com/problems/median-of-two-sorted-arrays/)

### 题目

There are two sorted arrays nums1 and nums2 of size m and n respectively.

Find the median of the two sorted arrays. The overall run time complexity should be O(log (m+n)).

You may assume nums1 and nums2 cannot be both empty.

**Example 1:**

> nums1 = [1, 3]
> nums2 = [2]
>
> The median is 2.0

**Example 2：**

> nums1 = [1, 2]
> nums2 = [3, 4]
>
> The median is (2 + 3)/2 = 2.5

### 思路

根据题目给的时间必须是O(log(m+n))，明白肯定是用分治法，但是没有想到具体的解题思路。只想到了简单解法，即把nums1和nums2归并到一个数组里，然后再返回中位数，但是复杂度是O(n+m),不符合要求。

#### 法一：分治法

首先了解下中位数的概念：

> 将一个集合划分为两个长度相等的子集，其中一个子集中的元素总是大于等于另一个子集中的元素。

然后，我们将A、B两个数据分别划分为两个部分，如：

```java
      left_A             |        right_A
A[0], A[1], ..., A[i-1]  |  A[i], A[i+1], ..., A[m-1]
```

```java
      left_B             |        right_B
B[0], B[1], ..., B[j-1]  |  B[j], B[j+1], ..., B[n-1]
```

将left_A和left_B放入一个集合，将right_A和right_B放入另一个集合，组成left_part和right_part：

```java
		  left_part          |        right_part
    A[0], A[1], ..., A[i-1]  |  A[i], A[i+1], ..., A[m-1]
    B[0], B[1], ..., B[j-1]  |  B[j], B[j+1], ..., B[n-1]
```

如果上述的情况下满足：

```java
1.len(left_part) = len(right_part) //注意这里只是作为一个前提条件去定义代码中的j变量，编程时不会去判断这个等式是否成立，所以不存在m+n为奇数时这个等式不成立的情况。
2.max(left_part) <= min(right_part)
即将A、B划分组合为相同长度的两部分，且其中一部分的元素总是大于等于另一部分。
```

那么A和B的共同中位数就为：

> medium = max(left_part)，n+m为奇数（划分时多出的那个元素划入左部）
>
> medium = (max(left_part) + min(right_part)) / 2，n+m为偶数

要确保这两个条件成立，只需要保证如下条件成立即可：

> （1）i + j = (m-i) + (n-j) +1（m-i + n-j + 1，这里加1是为了让m+n为奇数的时候left_part多一个元素，这样就可以直接返回max(left_part）的元素作为中位数，而当m+n为偶数的时候，加1也不会影响划分结果，即两边的元素个数依旧相等，具体看下面的代码实现。当然，不+1就是在总数为奇数的时候让右边多一个元素，处理方式一样。
>
> ​		设n>=m，则 j = (m+n+1)/2 - i，i = 0,1,...,m，这里是为了让len(left_part) = len(right_part)，选择元素个数少的数组作为划分的自变量，划分点i从0,1,...,m，这样的话可以减少划分的情况。因为要满足上面的等式，所以当i的划分点确定后，划分点j也是确定的。如下例所示：
>
> A = [0,3,4,5]
>
> B = [1,2,3,4,5,6,7,8]
>
> 当A中的划分点i = 2时，A被划分为两部分：[0,3,4]，[5]
>
> 这个时候为了满足len(left_part) = len(right_part)，所以B中的划分点 j 必须等于2，把B分为：[1,2,3]，[4,5,6,7,8]才能满足。所以选择数组元素少首先划分，可以减少划分的次数，对于m，划分的情况一共是0~m。

> （2）B[j-1] <= A[i] && A[i-1] <= B[j]
>
> 从（1）可知现在left_part和right_part的元素个数已经相等了（如果m+n为奇数，则左部多1个元素），但是还必须满足让左部的所有元素都小于等于右部的元素才行，即上式(2)。
>
> 如果不满足，则需要在m中寻找下一个切分点 i ，直到找到满足条件(2)的那个且分点，也就找到了问题的解。
>
> 当A[i-1] > B[j]时，表示左部中A部分存在一些比右部中大的元素，为了让A[i-1]小于B[j]，则需要将i向左移动；
>
> 当B[j-1] > A[i]时，表示左部中B部分存在比右部大的元素，为了让B[j-1]<=A[i]，则需要将 i 向右移动；

注意：找上述的划分点 i 不是顺序查找的，而是用二分查找。

代码如下：

```java
class Solution {
    public double findMedianSortedArrays(int[] nums1, int[] nums2) {
        int m = nums1.length;
		int n = nums2.length;
		if(m > n) {//to ensure m<=n
			int[] temp = nums1;nums1 = nums2;nums2 = temp;
			int tmp = m;m = n;n = tmp;
		}
		
		//halfLen的定义是求解问题的关键
        //
		int iMin = 0,iMax = m,halfLen = (m+n+1)/2;//这里的+1是为了保证当m+n为奇数时让left_part部分比left_right部分多一个元素，这样我们就可以直接返回max(left_part)，具体看下面的代码。
		while(iMin <= iMax) {
			//确定m数组的划分点，i表示把m中前i个元素划入左部；这个划分点用二分法寻找
			int i = (iMin + iMax)/2;
			//n数组的划分点，表示把j个元素划入左部
			int j = halfLen - i;
            
           //在进行大小的比较时，判断i和j是否越界,非常重要
			if((j!=0 && i!=m) && nums2[j-1] > nums1[i]) {
				iMin = i+1; //i is too small
			}else if((i!=0 && j!=n) && nums1[i-1]>nums2[j]) {
				iMax = i-1; //i is too big
			}else { //nums2[j-1]<=nums1[i]&&nums1[i-1]<=nums2[j]
				
                //此时已经找到了符合条件的i、j的切分位置，寻找左边最大的元素
                int maxLeft = 0;
				if(i==0)
					maxLeft = nums2[j-1];
				else if(j == 0)
					maxLeft = nums1[i-1];
				else
					maxLeft = Math.max(nums1[i-1], nums2[j-1]);
                if((m+n)%2==1)
                    return maxLeft;
				
				//注意在右半部分是求最小
				int minRight = 0;
				if(i==m)
					minRight = nums2[j];
				else if(j==n)
					minRight = nums1[i];
				else 
					minRight = Math.min(nums2[j], nums1[i]);
				
				return (maxLeft + minRight)/2.0;
				
			}//else
		}//while
		
		return 0.0;	
        
    }
}
```

时间复杂度为：O(log(min(m,n)))

### 法二：

其中的解法三是把该问题转换为求Top K问题。

https://leetcode-cn.com/problems/median-of-two-sorted-arrays/solution/xiang-xi-tong-su-de-si-lu-fen-xi-duo-jie-fa-by-w-2/

### 思考

1. 关于法一，自己其实有想到一部分，但是思路不清晰，无法完成地写出算法。这需要多锻炼才行。
2. 在第二次写代码的时候，最后的return语句分子本应该是2.0却写成了2，导致一直得不到正确的返回值。在代码的检查过程中，有想到是不是这句的原因，但是总认为这个语句很简单，应该不会写错，所以就没有认真的看。最后是在eclipse下一步步调试才发现问题出在此处。在debug时，任何一个语句都不能放过，出错的往往就是那些自认为不会写错的地方；此外，如果leetcode上实在找不出问题，就直接上eclipse一步步调试；最后，很多地方不要自己瞎想，直接看看他人的思路，这样会节省时间，也能获得新的思路。