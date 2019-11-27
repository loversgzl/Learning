

# Java-算法之链表

链表的各种操作：逆序（部分逆序，按某种条件逆序）、判断是否有环、环的入口节点、删除指定节点等。
链表的基本操作：1.在链表未插入一个节点，2.删除一个节点，
注意：存在空的情况，优先处理头结点等，

1. 两数相加 - 两个链表的简化操作
2. <a href="#简单选择排序">简单选择排序</a>

快捷键：Ctrl + Home 快速回到页面顶端查看目录，点击锚点，快速定位到算法。

### 链表的基本操作
```java
/**链表的基本结构
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) { val = x; }
 * }
 */

//在末尾添加一个节点,注意空的情况
public void addToTail(ListNode pHead, int value){
    ListNode pNew = ListNode(value);
    pNew.next = null;
    if(pHead == null)
        pHead = pNew;
    else{
        ListNode pNode = pHead
        while(pNode.next != null)
            pNode = pNode.next;
        pNode.next = pNew;
    }
}

//删除某个节点，注意头结点的处理
public void removeNode(ListNode pHead, int value){
    if(pHead == null)
        return;
    if(pHead.value == value)
        pHead = pHead.next;
    else{
        ListNode pNode = pHead;
        while(pNode.next != null && pNode.next.val != value)
            pNode = pNode.next;
        if(pNode.next != null && pNode.next.val == value)
            pNode.next = pNode.next.next;
    }
}

//反转链表
public ListNode reverseList(ListNode head) {
    ListNode prev = null;
    ListNode curr = head;
    while (curr != null) {
        ListNode nextTemp = curr.next;
        curr.next = prev;
        prev = curr;
        curr = nextTemp;
    }
    return prev;
}


```


### 两数相加
合并两个单链表
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
        ListNode root = new ListNode(0);
        ListNode p1 = l1, p2 = l2, curr = root;
        int carry = 0, temp = 0;
        while(p1 != null || p2 != null){
        	int x = (p1 != null) ? p1.val : 0;
        	int y = (p2 != null) ? p2.val : 0;
        	temp = x + y + carry;
        	carry = temp / 10;
        	curr.next = new ListNode(temp%10);
        	curr = curr.next;
        	if(p1 != null) p1 = p1.next;
        	if(p2 != null) p2 = p2.next;
        }

        if(carry > 0){
        	curr.next = new ListNode(1);
        }
        return root.next;
    }
}
```

### O(1)时间删除链表结点。
题目概述：给定链表某一个节点的指针，删除该节点。
解法：可以赋值下一个节点的值到本节点，然后删除下一个节点。
测试用例：如果删除是尾节点，那么只能从头遍历找前一个节点。
测试用例：如果是头结点，还需要将头结点设置为空。
测试用例：如果节点根本不在链表中，那么需要O(n)时间判断，所以为了满足要求，只能把这个考虑的问题交给函数调用者了。

```java
public void deleteNode(ListNode pHead, ListNode pDelete) {
	if(pDelete == null || pHead == null)
		return;
	//要删除的节点不是尾节点
	if(pDelete.next != null) {
		pDelete.val = pDelete.next.val;
		pDelete.next = pDelete.next.next;
	}//如果是尾节点，也是头节点
	else if(pHead == pDelete) {
		pHead = null;
		pDelete = null;
	}else {//如果是尾节点，但不是头节点
		ListNode pNode = pHead;
		while(pNode.next != pDelete)
			pNode = pNode.next;
		pNode.next = null;
	}
}
```

### 剑指offer：寻找倒数第 k 个节点 - 双指针操作
测试用例：空指针、节点小于k个、k小于等于0
相关题目：求链表的中间节点：双指针，一个一步，一个两步；
相关题目：判断一个单链表是否有环：同上，如果有环则两个指针会相遇。

```java
 public ListNode FindKthToTail(ListNode head,int k) {
	 if(head == null)
		 return null;
	 int gap = 1;
	 ListNode front = head, rear = head;
	 while(rear.next != null && gap < k) {
		 rear = rear.next;
		 gap++;
	 }
	 if(gap == k) {
		 while(rear.next != null) {
			 rear = rear.next;
			 front = front.next;
		 } 
		 return front;
	 }
	 return null;		 
}
```










