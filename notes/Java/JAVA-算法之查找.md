

# 查找-经典算法-Python

1. <a href="#二分查找">二分查找</a>
2. <a href="#快慢指针">快慢指针</a>
3. <a href="#经典结构">经典结构</a>：二叉搜索树、平衡二叉树、B树、红黑树
快捷键：Ctrl + Home 快速回到页面顶端查看目录，点击锚点，快速定位到算法。


**二分查找**<a name="二分查找"></a>
```java
/**条件：只能对已经排序好的列表进行查找
需求：对搜索时间要求为O(logn)一般都是二分查找。
推荐使用模板一，只要进行简单的修改，即可满足多种情况。
条件：left <= right
结束时：left 指向第一个不满足 if 条件中的值，
如if(array[mid] < target)：就是第一个大于等于target的值
如if(array[mid] <= target)：就是第一个大于target的值

问：在一个排序数组中寻找某个值，肯定存在这个值，如果不存在，则等同于下面查找第一个大于target的值！
答：使用 binarySearchOne() 进行二分查找。

问：在一个排序数组中寻找第一个大于 target 的值!
答：1、存在第一个大于 target 的值，2、target 即为最大值。
1.1、可以使用 binarySearchOne()，修改条件如下，最终left > right， 且 left 会指向第一个大于target的位置。
if(array[mid] <= targer) 
	left = mid + 1;
else
	right = mid - 1;
1.2、使用 binarySearchTwo()，最终 left == right,且指向第一个大于target的位置。

问：寻找第一个 大于等于 target的值！
答：使用 binarySearchOne()，修改如下，最终 left == right,且 left 指向第一个大于等于target的位置
if(array[mid] < targer) 
	left = mid + 1;
else
	right = mid - 1;
	
答：使用 binarySearchTwo()，修改条件如下，如果有多个target，可以找到左边界。
if(array[mid] < targer) 
	left = mid + 1;
else
	right = mid;
*/

/*模板一：用于查找数组中的某个确定值，经典用法，考察一般。
如果这个值不存在，则left会指向第一个比它大的值。！
*/
public int binarySearchOne(int[] array, int target) {
	int left = 0, right = array.length-1;
	while(left <= right) {
		int mid = (left+right)/2;
		if(array[mid] == target)
			return mid;
		if(array[mid] < target)
            left = mid + 1;
		else
		   right = mid - 1;
	}
	return -1;
}
    
/**模板二：查找第一个大于 target 值的坐标*/
public int binarySearchTwo(int[] array, int left, int right,int target){
        while(left < right){
            int mid = (left+right)/2;
//大于等于target要保留，因为可能就是这个坐标，小于则不可能。
            if(array[mid] <= target)
                left = mid + 1;
            else
                right = mid;
        }
//最后结束时有left==right，如果一定存在，则不需要以下语句，直接返回即可。
        if(array[left] >= target)
            return left;
        return -1;//如果target即是最大值，则返回-1；


    
//例题：查找左右边界，计算目标值的数量，使用了两次查找第一个最大值。
class Solution {
    public int search(int[] nums, int target) {
        return binarySearch(nums, target + 0.5) - binarySearch(nums, target - 0.5);
    }

    private int binarySearch(int[] nums, double target) {
        int left = 0, right = nums.length - 1;
        while (left <= right) {
            int mid = (right + left) >>> 1;
            if (nums[mid] < target) {
                left = mid + 1;
            } else if (nums[mid] > target) {
                right = mid - 1;
            }
        }
        return left;
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
2、平衡二叉树[AVL]：每个节点左右子树的深度差不超过1。
  平衡二叉树[AVL]缺点：对于插入删除频繁的操作，需要多次旋转，升级为红黑树，平衡二叉树性质的改进。
  平衡二叉树[AVL]缺点：每个节点只存放一个元素，最多只有两个子节点，查找需要多次磁盘IO（不同层数据存放在不同页），升级为B树。

**B 树**
B 树也称 B- 树,它是一颗多路平衡查找树。我们描述一颗B树时需要指定它的阶数，阶数表示了一个结点最多有多少个孩子结点，一般用字母m表示阶数。当m取2时，就是我们常见的二叉搜索树，查找不稳定，遍历比较麻烦，分支多层数少。B 树每一层存放了更多的节点，由 AVL 树的“瘦高”变成了“矮胖”，可以相对减少磁盘 IO 的次数，一般用于数据库中做索引，MongoDB 的索引就是用 B 树实现的。
B 树缺点：查找不稳定，遍历比较麻烦，升级为 B+ 树。
[B树参考博客](https://www.cnblogs.com/nullzx/p/8729425.html)

4、B+ 树：每个非叶子结点存放的元素只用于索引作用，所有数据保存在叶子结点。一般来说都会进行一个优化，就是将所有的叶子节点用指针串联起来，遍历叶子节  点就能获取全部数据，这样就能进行区间访问了，每个非叶子结点存放的元素只用于索引作用，所有数据保存在叶子结点。
[B+树参考](https://blog.csdn.net/wanderlustLee/article/details/81297253)

问：B树和B+树的区别？
答：两者最大的区别在于，B+树内部结点不保存数据，只用于索引，所有数据（或者说记录）都保存在叶子结点中。每个叶子结点都存有相邻叶子结点的指针，叶子结点本身依关键字的大小自小而大顺序链接。


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

