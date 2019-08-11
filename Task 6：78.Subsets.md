## Task 6：78.Subsets

### 题目

```
Given a set of distinct integers, nums, return all possible subsets (the power set).
Note: The solution set must not contain duplicate subsets.

Example:
Input: nums = [1,2,3]
Output:
[
  [3],
  [1],
  [2],
  [1,2,3],
  [1,3],
  [2,3],
  [1,2],
  []
]
```

### 思路

​		**注意：**任何问题，首先想到能不能暴力破解、即枚举每一种情况，但是同时问题又来了，如何枚举？？？具体怎么编码？？这就考虑到代码能力了。



**法一：枚举**

​		看了评论，有了一点思路，可以从头至尾遍历数组每个元素，每次在前一个元素的基础上加上当前元素组成一个子集，代码如下，但是下面的代码存在一个问题，就是这样的方式没有枚举出所有的情况。

```java
public List<List<Integer>> subsets(int[] nums) {
    List<List<Integer>> sol = new ArrayList<>();
    sol.add(new ArrayList());


    for(int i=1;i<=nums.length;i++){
        ArrayList<Integer> tmp = new ArrayList<Integer>();                
        for(int j=0;j<i;j++){
            tmp.add(nums[j]);
        }
        sol.add(tmp);
    }
    return sol;
}
```

运行结果：

```
输入
[1,2,3]
输出
[[],[1],[1,2],[1,2,3]]
预期结果
[[],[3],[2],[2,3],[1],[1,3],[1,2],[1,2,3]]
```

​	

​		在上面的代码的基础上，做如下修改，即遍历所有元素，每次都将新的元素插入到每一个已经生成的子集中，形成新的子集，直到数组的所有元素遍历完为止。

```java
class Solution {
    public List<List<Integer>> subsets(int[] nums) {
        List<List<Integer>> res = new ArrayList<>();
        res.add(new ArrayList<>()); //添加空集作为集合的子集
        
        for(int i=0;i<nums.length;i++){
            int all = res.size(); //当前集合中包含子集的个数
            for(int j=0;j<all;j++){ //将当前元素加入到当前集合的每个子集之中，形成新的子集
                List<Integer> tmp = new ArrayList<>(res.get(j)); //得到子集
                tmp.add(nums[i]); //将当前元素加入子集之中构成新的子集
                res.add(tmp); 
            }
        }
        return res;
    }
    
    /*递归枚举*/
}
```

**法二：位运算**

​		记得及时添加此处的[笔记](https://leetcode-cn.com/problems/subsets/solution/er-jin-zhi-wei-zhu-ge-mei-ju-dfssan-chong-si-lu-9c/)。



### 思考

1. 对于法一循环枚举的两种代码，思路是一样的，但是第二种能解题，第一种考虑的情况不全面，这种情况还是因为自己写的代码太少，逻辑理不清楚。