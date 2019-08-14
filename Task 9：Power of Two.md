## Task 9：[Power of Two](https://leetcode-cn.com/problems/power-of-two/)

### 题目

Given an integer, write a function to determine if it is a power of two.

```
Example 1:
Input: 1
Output: true 
Explanation: 2^0 = 1

Example 2:
Input: 16
Output: true
Explanation: 2^4 = 16

Example 3:
Input: 218
Output: false
```

### 思路

1. 一开始的比较傻，直接用n%2==0来判断，但是发现这只是判断n是否是2的倍数。
2. 然后意识到2的指数倍的数字的二进制表示都有一个共同点，那就是只包含一个1，比如说2^2=100(2)，2^5=100000(2)等。

#### 法一：

​		因此只需要计算一下n的二进制表示中1的个数即可，如果大于1个则return false。代码如下:

```java
class Solution {
    public boolean isPowerOfTwo(int n) {
        if(n <= 0)
            return false;
        if(n == 1)
            return true;
        
        //计算n的二进制表示里面有多少个1
        int count=0;
        while(n!=0){
            if((n&1) == 1)
                count++;
            n>>=1;
        }
        if(count>1)
            return false;
        else
            return true;
    }
}
```

#### 法二：

​		考虑到满足条件的数n的二进制表示中只有对应位为1，而(n-1)的二进制表示刚好与其相补，即存在（n&(n-1)==0）的情况，如：n=2^4=10000，而n-1=2^4-1=01111，代码如下：

```java
if(n<=0)
return false;
return ((n&(n-1)) == 0);
```

​		下面是极简写法，非常优秀的代码，必须学习模仿：

```java
class Solution {
    public boolean isPowerOfTwo(int n) {
        return n > 0 && (n & (n - 1))) == 0;
    }
}
```

### 思考

1. 看到上面代码的极简写法，非常的佩服，即优化了代码，又提高了效率，一定要学习这种coding风格，让代码优化到极致。

