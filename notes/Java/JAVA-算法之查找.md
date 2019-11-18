

# 查找-经典算法-Python

1. <a href="#二分查找">二分查找</a>
2. <a href="#快慢指针">快慢指针</a>
3. <a href="#经典结构">经典结构</a>：二叉搜索树、平衡二叉树、B树、红黑树
快捷键：Ctrl + Home 快速回到页面顶端查看目录，点击锚点，快速定位到算法。


**二分查找**<a name="二分查找"></a>
```java
/**
*二分查找
条件：只能对已经排序好的列表进行查找
需求：对搜索时间要求为O(logn)一般都是二分查找。
模板一：用于查找数组中的某个确定值，经典用法，考察一般。
模板二：有条件查找，如寻找第一个比5大的值，考察较多。
模板三：有双重条件的查找。
*/
public int binarySearchOne(int[] array, int target) {
	int left = 0, right = array.length-1;
	while(left <= right) {
		int mid = (left+right)/2;
		if(array[mid] == target)
			return mid;
		if(array[mid] > target)
			right = mid - 1;
		else
			left = mid + 1;
	}
	return -1;
}
    
//模板一：搜索旋转数组
//排序好的数组某一部分旋转了，查找某个数。
def search(nums, target):
    start,end = 0,len(nums) - 1
    while start <= end:
        mid = (start + end) // 2
        if nums[mid] == target:
            return mid
        if nums[mid] < nums[start]: # case [7 8 0 {1} 2 3 4 5]
            if nums[mid] <= target <= nums[end] :
                start = mid + 1
            else:
                end = mid - 1
        else:# nums[mid] > nums[start] case [4 5 6 {7} 0 1 2]
            if nums[start] <= target <= nums[mid]:
                end = mid - 1
            else:
                start = mid + 1
    return -1    
    
/**
*模板二：查找第一个大于等于 target 值的坐标，或者查找最后一个比 target 小的值的坐标。
其实上述两个问题的坐标是相邻的，但前者比后者的编码更方便处理一些，可以找前者-1得到后者

*/
public int binarySearchTwo(int[] array, int left, int right,int target){
//寻找第一个大于等于target的值，找不到返回 -1.
        while(left < right){
            int mid = (left+right)/2;
//大于等于target要保留，因为可能就是这个坐标，小于则不可能。
            if(array[mid] >= target)
                right = mid;
            else
                left = mid+1;
        }
//最后结束时有left==right，如果一定存在，则不需要以下语句，直接返回即可。
        if(array[left] >= target)
            return left;
        return -1;
/*
同理，该语句也可改为寻找最后一个小于target的值，如果更改while语句可能造成死循环，
即判断条件改为如下，最后得到3，4都小于target，则陷入死循环。
if(array[mid] < target) 
	left = mid; 
else 
	right = mid-1;
所以可以在还是找第一个最大值，return left - 1，即可
*/
}

//Tencent2018秋招
/*
小Q的父母要出差N天，走之前给小Q留下了M块巧克力。小Q决定每天吃的巧克力数量不少于前一天吃的一半，但是他又不想在父母回来之前的某一天没有巧克力吃，请问他第一天最多能吃多少块巧克力，二分查找的变形。
*/
import java.util.Scanner;
public class Main{
    /*
    统计第一天吃了s，后面严格克制自己只刚好满足要求，而不多吃。
    如：14、7、4、2、1，但是我们可以吃，14、8、4、2、1
    */
    public static int mySum(int n, int s){
        int total = 0;
        for(int i=0; i<n; i++){
            total += s;
            s = (s+1)/2;
        }
        return total;
    }
    public static void main(String[] args) {
        Scanner scan = new Scanner(System.in);
        int n = scan.nextInt();//父母出差的天数
        int m = scan.nextInt();//输入巧克力的总数
        int left=1, right=m;
//二分查找，寻找第一天吃了 mid 就不够的情况（同第一个大于 5 的数）。
        while(left < right){
            int mid = (left+right)/2;//如果第一天吃一半
//第一天吃了 mid 大于等于给定值（同大于等于 5）
            if(mySum(n,mid) >= m){
            	right = mid;
            }else{
                left = mid+1;
            }
        }
//最后进行判断，如果是大于，那么肯定是第一次大于，否则就是等于或者小于。
        if(mySum(n,left) > m)
            System.out.println(left-1);
        else
        	System.out.println(left);
    }
}
```

**快慢指针**<a name="快慢指针"></a>
```java
/**
*快慢指针
寻找环入口：从起点开始，走到某处会出现一个环的入口，找出该点。
快指针比慢指针快两倍，同时出发，肯定会在某一点 C 相遇，慢指针走了 n，快指针走了 2n，因此从 C 点，慢指针再走 n 步是不是和快指针走的一样远。解释：只要在环内，就一定会相遇。
*/

```

**经典结构**<a name="经典结构"></a>
**二叉搜索树、平衡二叉树、B 树、红黑树**
1、二叉搜索树：左边的值小于根节点，右边的值大于根节点。
  二叉搜索树缺点：会出现全部节点在一端的情况，升级为平衡二叉树[AVL]。
2、平衡二叉树[AVL]：根节点左右两边深度差不超过1。
  平衡二叉树[AVL]缺点：对于插入删除频繁的操作，需要多次旋转，升级为红黑树，平衡二叉树性  质的改进。
  平衡二叉树[AVL]缺点：每个节点只存放一个元素，最多只有两个子节点，查找需要多次磁盘IO（不同层数据存放在不同页），升级为B树。

**B 树**
B 树也称 B- 树,它是一颗多路平衡查找树。我们描述一颗B树时需要指定它的阶数，阶数表示了一个结点最多有多少个孩子结点，一般用字母m表示阶数。当m取2时，就是我们常见的二叉搜索树。
3、B 树也叫 B- 树，一种多路平衡树。查找不稳定，遍历比较麻烦，分支多层数少。B 树每一层存放了更多的节点，由 AVL 树的“瘦高”变成了“矮胖”。可以相对减少磁盘 IO 的次数。一般用于数据库中做索引，MySQL、MongoDB 的索引就是用 B 树实现的。
B 树缺点：查找不稳定，遍历比较麻烦，升级为 B+ 树。


[B树参考博客](https://www.cnblogs.com/nullzx/p/8729425.html)


4、B+ 树：每个非叶子结点存放的元素只用于索引作用，所有数据保存在叶子结点。一般来说都会进行一个优化，就是将所有的叶子节点用指针串联起来，遍历叶子节  点就能获取全部数据，这样就能进行区间访问了，每个非叶子结点存放的元素只用于索引作用，所有数据保存在叶子结点。
[B+树参考](https://blog.csdn.net/wanderlustLee/article/details/81297253)

5、红黑树：[参考博客](https://blog.csdn.net/net_wolf_007/article/details/79706498)
由来：对平衡二叉树的限制条件，通过对任何一条从根到叶子的简单路径上各个节点的颜色进行约束，确保没有一条路径会比其他路径长2倍，因而是近似平衡的。所以相对于严格要求平衡的AVL树来说，它的旋转保持平衡次数较少。用于搜索时，插入删除次数多的情况下我们就用红黑树来取代AVL。
规则：
特征一：节点要么是红色，要么是黑色（红黑树名字由来）。
特征二：根节点是黑色的
特征三：每个叶节点(nil或空节点)是黑色的。
特征四：每个红色节点的两个子节点都是黑色的（相连的两个节点不能都是红色的）。
特征五：从任一个节点到其每个叶子节点的所有路径都是包含相同数量的黑色节点。


```python
'''二叉搜索树
如果满足：(L.val-root.val)*(R.val-root.val) <= 0，那么LR肯定在根节点两侧。
1、二叉搜索树查找某个值
2、普通树查找某个值
3、中序遍历的方式将二叉搜索树转为数组
4、判断是否为平衡二叉树
'''
def SearchOne(root, val):
    while root:
    	if root.val == val:
    		return root
    	elif root.val > val:
    		root = root.left
    	else:
    		root = root.right
    return None

#寻找某个值，用迭代的方式遍历所有节点，适用于所有树
def SearchTwo(root, val):
	stack = [root]
	while stack:
		node = stack.pop(0)
		if node == None:
			continue
		if node.val == val:
			return node
		stack.append(node.left)
		stack.append(node.right)
	return None

#中序遍历的方式将二叉搜索树转为数组
def traverse(root):
	if root == None:
		return []
	return traverse(root.left) + [root.val] + traverse(root.right)

#判断是否为平衡二叉树
def isBalanced(root):
	def dfsBalanced(node):
	    if not node: return 0 #返回数据0
	    left = self.dfsBalanced(node.left)
	    right = self.dfsBalanced(node.right)
	    if left == -1 or right == -1 or abs(left-right)>1:
	        return -1
	    return max(left, right)+1#注意返回条件的选择
    return self.dfsBalanced(root) >= 0
```

