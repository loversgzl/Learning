

# JAVA-算法之树

### 遍历
**基础知识（树的算法，一看就会，一写就废）**：
前中后，知道两种，算另外一种。肯定给出中序遍历，否则无法确定一棵树。
知前中：根据前序遍历寻找根节点，就是最前面的，然后中序遍历判断左右子节点（左右两边）。
知中后：根据后序遍历寻找根节点，就是最后的，然后在中序遍历根节点的左右两边就是左右子树。
可参考：前序遍历：ABCDEFGHK，中序遍历：BDCAEHGKF，后序遍历：DCBHKGFEA

**基础知识**：一、遍历二叉树（递归+迭代）、求二叉树的高度与宽度、

```java
//先序遍历，递归
List<Integer> list = new LinkedList<>();
public List<Integer> preorderTraversal(TreeNode root) {
	if(root !=null){
        list.add(root.val);
        preorderTraversal(root.left);
        preorderTraversal(root.right);
	}
	return list;    
}
//先序遍历，迭代用栈做
public List<Integer> preorderTraversal(TreeNode root) {
    Stack stack = new Stack();
    List<Integer> nums = new ArrayList<>();
    stack.push(root);
    while(!stack.empty()){
        TreeNode temp = (TreeNode)stack.pop();
        if(temp != null){
            nums.add(temp.val);
            stack.push(temp.right);
            stack.push(temp.left);
        }
	}
    return nums;
}

//求二叉树的深度，递归，迭代用队列来做，入队记录每一层的节点数，出队。
public int maxDepth(TreeNode root) {
    if (root == null) {
      return 0;
    } else {
      int left_height = maxDepth(root.left) + 1;
      int right_height = maxDepth(root.right) + 1;
      return Math.max(left_height, right_height);
    }
}

public int maxDepth(TreeNode root) {
    if (root == null) return 0;
    return Math.max(maxDepth(root.left), maxDepth(root.right)) + 1;
}

//求根节点到最近叶子节点的距离
public int minDepth(TreeNode root) {
    if(root == null) return 0;
    int leftDepth = minDepth(root.left);
    int rightDepth = minDepth(root.right);
    return root.left==null || root.right==null ? leftDepth+rightDepth+1:Math.min(leftDepth, rightDepth)+1;
}

/*
求二叉树的宽度，即节点数最多的那一层节点数；
设置100层的数组存储节点数，k表示层数，初始为0；
*/
int[] count = new int[100];
int MAX = -1;
public int FindWidth(TreeNode root, int k){
    if( root == null)  
        return;
    count[k]++;
    if(MAX<count[k]) 
        MAX=count[k];
    FindWidth(root.lchild,k+1);//这里不要用k++，会改变下面k的值。
    FindWidth(root.rchild,k+1);
}

/*DFS：可细分为先序，中序，后序遍历
*/


/*BFS
*/



```




1. <a href="#简单选择排序">知先序中序重构二叉树</a>

快捷键：Ctrl + Home 快速回到页面顶端查看目录，点击锚点，快速定位到算法。

```java
/**
 * Definition for binary tree
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */
import java.util.HashMap;
public class Solution {
    public TreeNode reConstructBinaryTree(int [] pre,int [] in) {
        if(pre == null || in == null)
            return null;
        HashMap<Integer,Integer> map = new HashMap<>();
        for(int i=0;i<in.length;i++)
            map.put(in[i],i);
        return preIn(pre,0,pre.length-1,in,0,in.length-1,map);
    }
     
    public TreeNode preIn(int[] p,int pi,int pj,int[] n,int ni,int nj,HashMap<Integer,Integer> map){
        if(pi > pj)
            return null;
        TreeNode head = new TreeNode(p[pi]);
        int index = map.get(p[pi]);
        head.left = preIn(p,pi+1,pi+index-ni,n,ni,index-1,map);
        head.right = preIn(p,pi+index-ni+1,pj,n,index+1,nj,map);
        return head;
    }
}
```