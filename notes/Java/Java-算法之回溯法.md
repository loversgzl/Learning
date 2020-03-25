

# 回溯法-经典算法-Python
**简介**：
回溯法（探索与回溯法，树上的DFS）是一种选优搜索法，又称为试探法，按选优条件向前搜索，以达到目标。但当探索到某一步时，发现原先选择并不优或达不到目标（剪枝函数），就退回一步重新选择。
函数中先确定必要的参数，再写递归函数（函数内，如果在函数外需要注意全局变量传递的问题），最后再调用，否则可能会出现调用在声明前的错误。
递归函数的编写注意事项：
一、参数的选取，回溯需要哪些参数，因为是函数内的函数，因此不用考虑全局变量的传递问题。
二、结束条件，到什么时候终止，如果写错了，很容易造成无限递归。
三、如果递归不成功，是否需要重置状态。

1. <a href="#老鼠走迷宫">老鼠走迷宫</a>
2. <a href="#统计小岛的最大面积">统计小岛的最大面积</a>
3. <a href="#骑士走棋盘">骑士走棋盘</a>
4. <a href="#八皇后">八皇后</a>
5. <a href="#正则表达式匹配">正则表达式匹配</a>

快捷键：Ctrl + Home 快速回到页面顶端查看目录，点击锚点，快速定位到算法。

往四个方向走；
```java
int[] dr = [0,0,1,-1];
int[] dc = [1,-1,0,0];
//r,c为任意位置
for(int k=0; k<4; k++){
	int nr = r+dr[k];
	int nc = c+dc[k];
	if (0 <= nr && nr < N && 0 <= nc && nc < N) {xxx};
}
```



* **老鼠走迷宫**<a name="老鼠走迷宫"></a>
题目概述：给定一个二维数组，最外层是围墙，全为 1，里面 0 表示可以通行，寻找一条从左上角[0,0]到右下角的可行路线[n-1,n-1]。
解题思路：老鼠往四个方向探索，如果其中一个走不通，则退回来走另一个方向。
进阶：如果迷宫中有多条路径，显示所有路径，对代码进行简单修改即可。
```python
class Solution {
    public boolean exist(int[][] maze) {
    	if(dfs(maze,0,0)) 
        	return true;
    	return false;
    }
    public boolean dfs(int[][] maze, int i, int j) {
    	if(i<0||j<0||i>=maze.length||j>=maze[0].length||maze[i][j] == 1) return false;
    	if(i == maze.length-1 && j == maze[0].length-1) return true;
    	board[i][j] = 1;
    	boolean res = (dfs(maze,i+1,j)||dfs(maze,i-1,j)||dfs(maze,i,j+1)||dfs(maze,i,j-1));
    	board[i][j] = 0;
    	return res;
    }
}
```

* **统计小岛的最大面积**<a name="统计小岛的最大面积"></a>
题目概述：二维数组，统计1连续的最大面积
解题思路：


* **正则表达式匹配**<a name="正则表达式匹配"></a>
  题目概述：给你一个字符串 s 和一个字符规律 p，请你来实现一个支持 '.' 和 '*' 的正则表达式匹配。

  '.' 匹配任意单个字符
  '*' 匹配零个或多个前面的那一个元素

  解题思路：注意一：空的情况；注意二：*可以将前一个字符匹配为零，或者多个。官方解题采用回溯的思想。
  
```java
class Solution{
	 public boolean isMatch(String text, String pattern) {
		 if(pattern.isEmpty()) return text.isEmpty(); //如果pattern为空的情况
		 boolean first_match = (!text.isEmpty() && 
				 (pattern.charAt(0)==text.charAt(0)||pattern.charAt(0)=='.'));
		 //下面采用了回溯的方法
		 if(pattern.length()>=2 && pattern.charAt(1)=='*')
			 return (isMatch(text,pattern.substring(2)) || 
					 (first_match && isMatch(text.substring(1),pattern)));
		 else
			 return (first_match && isMatch(text.substring(1),pattern.substring(1)));
	 }	
}
```


