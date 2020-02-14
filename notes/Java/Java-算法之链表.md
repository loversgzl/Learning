

# Java-算法之链表

链表的各种操作：逆序（部分逆序，按某种条件逆序）、判断是否有环、环的入口节点、删除指定节点等。
链表的基本操作：1.在链表未插入一个节点，2.删除一个节点，
注意：链表通常可以衍生出循环链表，静态链表，双链表等。对于链表使用，需要注意头结点的使用，存在空的情况，优先处理头结点等，
链表与数组的比较：链表在插入的时候可以达到 O(1) 的复杂度，比另一种线性表 —— 顺序表快得多，但是查找一个节点或者访问特定编号的节点则需要O(n) 的时间，而顺序表相应的时间复杂度分别是O(log n) 和 O(1)。

1. <a href="#链表的基本操作">链表的基本操作</a>
2. <a href="#合并两个单链表">合并两个单链表之 两数相加 和 合并排序链表</a>
3. <a href="#O(1)时间删除链表结点">O(1)时间删除链表结点</a>
4. <a href="#双指针操作"> 寻找倒数第 k 个节点 - 双指针操作</a>
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
        if(pNode.next != null)
            pNode.next = pNode.next.next;
    }
}

//反转链表-迭代方式
public ListNode reverseList(ListNode pHead) {
    if(pHead == null || pHead.next == null)
    		return pHead;
    ListNode prev = null;
    ListNode curr = pHead;
    while (curr != null) {
        ListNode nextTemp = curr.next;
        curr.next = prev;
        prev = curr;
        curr = nextTemp;
    }
    return prev;
}

//递归方式
public ListNode reverseList(ListNode head) {
    if (head == null || head.next == null) return head;
    ListNode p = reverseList(head.next);
    head.next.next = head;
    head.next = null;
    return p;
}

```


### 两数相加  
<a name="合并两个单链表">合并两个单链表</a>

```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) { val = x; }
 * }
 */
//两个单链表相加
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

//合并两个单链表，递增排序，递归+迭代的方式

//递归方式
public ListNode mergeTwoLists(ListNode l1, ListNode l2) {
    if(l1 == null)
        return l2;
    if(l2 == null)
        return l1;
    if(l1.val <= l2.val) {
        l1.next = mergeTwoLists(l1.next, l2);
        return l1;
    }else {
        l2.next = mergeTwoLists(l1, l2.next);
        return l2;
    }
}

//迭代方式
public ListNode mergeTwoLists(ListNode l1, ListNode l2) {
    ListNode newHead = new ListNode(-1);
    ListNode pre = newHead;
    while(l1 != null && l2 != null) {
        if(l1.val <= l2.val) {
            pre.next = l1;
            l1 = l1.next;
        }else {
            pre.next = l2;
            l2 = l2.next;
        }
        pre = pre.next;
    }
    pre.next = l1==null ? l2 : l1;
    return newHead.next;
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
<a name="双指针操作"></a>
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

//快慢指针寻找链表的中间节点


//只要进入到环内，则两个指针一定会相遇。
//理解：快指针一定会跑到慢指针后面，如果相邻则下一次相遇，如果空一格，则变第一种情况，依次类推。
public boolean hasCycle(ListNode head) {
    if(head == null || head.next==null)
        return false;
    ListNode oneStep = head;
    ListNode twoStep = head.next;
    while(oneStep != twoStep) {
        if(twoStep == null || twoStep.next == null)
            return false;
        oneStep = oneStep.next;
        twoStep = twoStep.next.next;
    }
    return true;
}
```










