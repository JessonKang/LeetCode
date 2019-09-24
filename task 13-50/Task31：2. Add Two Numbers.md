## Task31：[2. Add Two Numbers](https://leetcode-cn.com/problems/add-two-numbers/)

### 题目

You are given two non-empty linked lists representing two non-negative integers. The digits are stored in reverse order and each of their nodes contain a single digit. Add the two numbers and return it as a linked list.

You may assume the two numbers do not contain any leading zero, except the number 0 itself.

**Example:**

> Input: (2 -> 4 -> 3) + (5 -> 6 -> 4)
> Output: 7 -> 0 -> 8
> Explanation: 342 + 465 = 807.

### 思路

开辟一个新的链表头结点用于指向合并后的链表；设两个指针分别指向两个链表的第一个节点，遍历两个链表，并把对应链表的节点之和作为新节点的val，将新节点链入到返回链表的尾部，注意进位的情况，代码如下：

**代码**：自己写的

```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) { val = x; }
 * }
 */
class Solution {
    public ListNode addTwoNumbers(ListNode l1, ListNode l2) {
        ListNode p1=l1,p2=l2;
        ListNode res = new ListNode(0); //建立头结点，便于操作
        ListNode tail = res; //尾插法建立链表，tail指向链尾
        int carry = 0; //进位标识
        int sum = 0; 
        //p1和p2的指向都不为空
        while(p1!=null && p2!=null){
            sum = p1.val + p2.val;
            if(carry == 1)
                sum ++;
            if(sum >= 10){
                carry = 1;
                sum %= 10;
            }else
                carry = 0;
            
            //carry = (sum >= 10 ? 1 : 0);
            
            ListNode p = new ListNode(sum);
            tail.next = p;
            tail = p;
            
            p1 = p1.next;p2 = p2.next;
        }
        //p2不为空，将p2的结点值插入res中，注意是否有之前的运算产生的进位
        while(p1 != null){ //
            sum = p1.val;
            if(carry == 1)
                sum++;
            if(sum >= 10){
                carry = 1;
                sum %= 10;
            }else
                carry = 0;
            
            tail.next = new ListNode(sum);
            tail = tail.next;
            p1 = p1.next;
        }
        //p1不空，...
        while(p2 != null){
            sum = p2.val;
            if(carry == 1)
                sum++;
            if(sum >= 10){
                carry = 1;
                sum %= 10;
            }else
                carry = 0;
            
            
            tail.next = new ListNode(sum);
            tail = tail.next;
            p2 = p2.next;
        }
        //p1与p2都已遍历完毕，但是要注意是否有产生的进位
        if(carry == 1){
            tail.next = new ListNode(1);
        }
        return res.next;
    }
}
```

代码：速度最快的版本

```java
public ListNode addTwoNumbers(ListNode l1,ListNode l2){
    ListNode head = new ListNode(0);
    ListNode result = head;
    int temp = 0;
    while(l1 != null || l2 != null){
        int val1 = l1==null ? 0:l1.val;
        int val2 = l2==null ? 0:l2.val;
        int val = val1 + val2 + temp;
        temp = val / 10; //保存进位信息
        val = val % 10;
        
        result.next = new ListNode(val);
        result = result.next;
        
        if(l1 != null) l1 = l1.next;
        if(l2 != null) l2 = l2.next;
    }
    if(temp > 0){
        result.next = new ListNode(temp);
    }
    return head.next;
}
```

### 思考

同样的解题思路，但是第二种代码的写法明显更加简洁、易读，需要多学习、模仿。





