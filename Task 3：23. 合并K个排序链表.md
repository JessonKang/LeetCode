## Task 3：23. 合并K个排序链表

注意：任何问题，都首先看看能不能用暴力解法，因为第一要义是要先解出问题，最优解法都是建立在能解出题目的基础上的。

### 题目

合并 *k* 个排序链表，返回合并后的排序链表。请分析和描述算法的复杂度。

**示例:**

```
输入:
[
  1->4->5,
  1->3->4,
  2->6
]
输出: 1->1->2->3->4->4->5->6
```

### 思路

#### 法一：分治法

​		可以先把问题简单化，合并K个有序链表，最简单的情况就是合并2个链表。所以可以用分治思想来解决这个问题，类似于归并排序，先把K个链表两两合并，然后再往上层回溯，最后K个链表全部合并完毕。

```java
class ListNode{
	int val;
	ListNode next=null;
	ListNode(int val) {
		this.val = val;
	}
}
	
public class task3_MergeKList {
	public ListNode mergeKLists(ListNode[] lists) {
		if(lists.length == 0)
			return null;
		//归并
		return solve(lists,0,lists.length-1);
	}
	
	private ListNode solve(ListNode[] arr,int left,int right) {
		if(left == right)//递归到只有一个列表时
			return arr[left];
		
		int mid = (left + right) >> 1;
		ListNode lnode = solve(arr,left,mid);
		ListNode rnode = solve(arr,mid+1,right);
		return merge(lnode,rnode);
	}
	
	//合并两个有序链表的递归写法(！！！！！没看懂！！！！！)
	private ListNode merge(ListNode node1,ListNode node2) {
		if(node1 == null)
			return node2;
		if(node2 == null)
			return node1;
		if(node1.val < node2.val) {
			node1.next = merge(node1.next,node2);
			return node1;
		}else {
			node2.next = merge(node1,node2.next);
			return node2;
		}
	}
    
    //合并两个有序链表的非递归写法
	private ListNode merge1(ListNode node1,ListNode node2) {
		if(node1 == null)
			return node2;
		if(node2 == null)
			return node1;
		
        ListNode dummyHead = new ListNode(0);
        ListNode curr = dummyHead;
        while(node1!=null && node2!=null){
            if(node1.val < node2.val){
                curr.next = node1;
                curr = curr.next;
                node1 = node1.next;
            }else{
                curr.next = node2;
                curr = curr.next;
                node2 = node2.next;
            } 
        }
        
        if(node1==null) curr.next = node2;
        if(node2==null) curr.next = node1;
        
        return dummyHead.next;
	}	
}
```

#### 法二：借助优先级队列

​		首先在K个有序链表中各取第一个节点共k个节点建一个优先级队列，然后将队首元素（最小）出队至保存结果的链表中，然后继续在出队节点所在的链表中取后续节点（该链表不空）放入队列中。如此往复，直至队列为空。

代码如下：

```java
class Solution {
   public ListNode mergeKLists(ListNode[] lists) {
       if(lists.length == 0) return null;
       
       //保存结果的新链表
       ListNode dummyHead = new ListNode(0);
       ListNode curr = dummyHead;
       
       PriorityQueue<ListNode> pq = new PriorityQueue<>(new Comparator<ListNode>(){
          @Override
           public int compare(ListNode o1,ListNode o2){
               return o1.val - o2.val;
           }
       });
       
       for(ListNode list:lists){
           if(list == null)
               continue;
           pq.add(list);
       }
       
       while(!pq.isEmpty()){
           ListNode nextNode = pq.poll();//最小的节点出队，该节点保存了指向下一个节点的next，所以便于用来添加下一个入队的节点
           curr.next = nextNode;
           curr = curr.next;
           if(nextNode.next != null){
               pq.add(nextNode.next);
           }
       }
       return dummyHead.next;
   }
}

```

### 思考：

​	1.递归调用的过程确实是相当难以理解，这方面一定要重点突破才行；

​	2.对集合的各种操作不了解，如PriorityQueue的Comparator。
​	3.在刷题的过程中用到了很多基础的数据结构和基础算法，可以顺便把这些代码整理到一起！！！