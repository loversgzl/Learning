

# 动态规划-Python

要解诸如最佳问题，使用动态规划，一般寻找唯一的情况要用到贪心，但是如果发现有规律可寻找，那么就改进为dp。理解动态规划最重要的是找一个实例，自已从头到尾推一下，推完就明白了。

1. <a href="#01背包问题">01、完全、混合背包问题</a>
2. <a href="#最小花费爬楼梯">最小花费爬楼梯</a>
3. <a href="#买卖股票的最佳时机1-5">买卖股票的最佳时机1-5</a>
4. <a href="#打家劫舍">打家劫舍</a>
5. <a href="#石子游戏">石子游戏</a>
6. <a href="#三色旗">三色旗</a>
7. <a href="#摆动排序">摆动排序</a>
8. <a href="#煎饼排序">煎饼排序</a>
9. <a href="#距离相等的条形码">距离相等的条形码</a>
10. <a href="#距离顺序排列矩阵单元格">距离顺序排列矩阵单元格</a>

快捷键：Ctrl + Home 快速回到页面顶端查看目录，点击锚点，快速定位到算法。

<a name="01背包问题"></a>
**01、完全、混合背包问题**
```python
'''背包问题
1-01背包 
问题概述：这里注意，物品只能选择0个或1个。
状态转移方程：需要倒过来range(capacity, weights[i]-1, -1),这样才是01背包
2-完全背包 
问题概述：给定物品的种类，每种物品的数量任意，达到最大价值。
状态转移方程：从range(weights[i], capacity+1)那么数量是不作任何限制的，可用来做完全背包问题。
3-混合背包 
问题概述：给定物品的种类，每种物品的数量有限制，达到最大价值。
可将物品数量变成1，转换为01背包问题。
number=5是物品的数量，capacity=10是书包能承受的重量
weights=[2,2,6,5,4]是每个物品的重量，prices=[6,3,5,4,6]是每个物品的价值
01背包经典应用：
1-外卖满减
'''
def knapsack01(capacity, number, weights, prices):
    item = [0]*(capacity+1)
    value = [0]*(capacity+1)#每个重量所能达到的最大价值。
    for i in range(number):#遍历每一个水果
    #前面的重量+这个物品的重量=后面的重量，如果价值更高，则替换掉。
        for x in range(capacity, weights[i]-1, -1):
            w = x - weights[i]
            newvalue = value[w] + prices[i]#w重量的价值最优解+当前的价值，是否比x重量的最优解大。
            if newvalue > value[x]:
                value[x] = newvalue
                item[x] = i#记录这个重量的最优解是：前面最优解+这个水果
        print(i,value)

    i = capacity
    while i > 0:
        print("编号：%d  价格：%d "%(item[i],prices[item[i]]))
        i = i - weights[item[i]]

knapsack01(10, 6, [2,2,3,1,5,2], [2,3,1,5,4,3])
```

<a name="最小花费爬楼梯"></a>
**最小花费爬楼梯**
```python
'''最小花费爬楼梯
问题描述：i代表楼梯，数值代表消耗的体力，一次可以跨一级或者两级台阶，求消耗最少的体力到达终点。
cost = [1, 100, 1, 1, 1, 100, 1, 1, 100, 1]
'''
def minCostClimbingStairs(cost):
    prv1, prv2 = 0, 0
    for i in range(2,len(cost) + 1):
        #从2开始，比较前两级台阶。暂时并没有考虑后面的台阶。
        #从整体考虑：prv2是目前最优解+刚刚才移动的一个位置
        #和上次最优解prv1+跳到中间节点（移动一个位置后面的一个值）
        #不要问为什么prv1跳前面的点，prv2跳后面的点，其实这个是一个值，与后面的每两个点都要相加，然后比较。
        prv1,prv2 = prv2,min(prv1 + cost[i-2],prv2 + cost[i-1])
    return prv2
```

<a name="买卖股票的最佳时机1-5"></a>
**买卖股票的最佳时机1-5**
```python
'''买卖股票的最佳时机1-5
参数：股票价格：[7,1,5,3,6,4]
I:如果你最多只允许完成一笔交易，即买一次股票，卖一次股票。
II：你可以尽可能地完成更多的交易，买之前需卖出之前购买的股票。
III:你最多可以完成两笔交易
IV：你最多可以完成 k 笔交易。三维dp
V：你可以尽可能地完成更多的交易，卖出股票后，你无法在第二天买入股票 (即冷冻期为 1 天)。二维dp
'''
def One(prices):
    if len(prices) < 2:
        return 0
    minimum,profit= prices[0],0
    for x in prices:
        minimum = min(x, minimum)
        profit = max(x - minimum, profit)
    print(profit)

def Two(prices):
    if len(prices) < 2:
        return 0
    profit = 0
    for i in range(1, len(prices)):
        p = prices[i] - prices[i-1]
        if p > 0:
            profit += p
    print(profit)

def Three(prices):
    buy1 = buy2 = -10**10
    profit1 = profit2 = 0
    #其实就是将第一笔的利益，传递到第二笔利益的变量上，第二笔变量则时刻记录前两笔价值最高的利益。
    for price in prices:
        buy1 = max(buy1, -price)#每次比较买入的价格，记录最低的那个
        profit1 = max(profit1, buy1+price)#以最低价格买入的股票，和每个价格卖出时的差值，记录最大那个
        buy2 = max(buy2, profit1-price)#拿第一笔赚来的钱减去第二笔的购买资金，找到剩余最多的那笔
        profit2 = max(profit2, buy2+price)#由于上面剩余最多，那么和后面所有价格之和最大的那个就是两笔资金的最大值
    return profit2
```

<a name="打家劫舍"></a>
**打家劫舍**
```python
'''打家劫舍
财富数量：[1,2,3,1]，求能获取的最大财富值。
测试用例：注意，空，一个值，两个值，等情况。

I:如果两间相邻的房屋在同一晚上被小偷闯入，系统会自动报警。
II：房屋都围成一圈，如果两间相邻的房屋在同一晚上被小偷闯入，系统会自动报警。
III:房屋连成一颗二叉树，如果两间相邻的房屋在同一晚上被小偷闯入，系统会自动报警。
'''
def robOne():
    if not nums:
        return 0
    elif len(nums) <= 2:
        return max(nums)
    #[0,0]其实是两条偷窃路线
    l = [[0,0] for _ in range(len(nums))]
    for i,n in enumerate(nums):
        #i-1是-1，列表里也是可以的。
        l[i][0] = l[i-1][1] + n #（上上一家保存下来的最大值，这里是两个上 + 自己价值的值）
        l[i][1] = max(l[i-1][1],l[i-1][0])#我这家不能偷，之前偷窃的最大值保存进来。
    return max(l[-1][0],l[-1][1])

def robTwo(nums):
    n = len(nums)
    if n == 0:
        return 0
    if n == 1:
        return nums[0]
    
# nums[0] was robbered，经过统计，fst1是不加最后一个值时最大财富，sed1是加了最后一个值的财富
    fst1, sed1 = nums[0], nums[0]
# nums[0] was not robbered，类似上，
    fst2, sed2 = 0, nums[1]
    
    #分两种情况分别统计最大的值
    for i in range(2, n):
        nxt1 = max(fst1 + nums[i], sed1)#统计最大值
        fst1 = sed1#通过一个中间值sed1，延缓一个记录
        sed1 = nxt1
        
        nxt2 = max(fst2 + nums[i], sed2)
        fst2 = sed2
        sed2 = nxt2
        
    return max(fst1, sed2)

def robThree(nums):
    if root==None:
        return 0
    def helper(root):
        if root==None:
            return [0,0]
#第一个值为 上上次偷窃的最大值+这家偷窃的值，第二个值为到上次为止，偷窃的最大值。
        left = helper(root.left)
        right = helper(root.right)
        rob = root.val + left[1] + right[1]
        skip = max(left) + max(right)
        return [rob, skip]
    res = helper(root)
    return max(res)
```

石子游戏<a name="石子游戏"></a>
```python
'''石子游戏
题目简介：有一堆石子排成一行，piles[i] 都是正整数，两人轮流从两端拿，问最终先手比后手会多多少颗石子。
解题详解：dp其实就是存储了递归过程中的数值，dp[i][j]代表从i到j所能获得的最大的绝对分数，（比如为1就说明先手从 i 到 j 可以赢后手1分），如何计算dps[i][j]呢:max(piles[i]-dp[i+1][j],piles[j]-dp[i][j-1]);
这里减去dp数组是因为后手也要找到最大的
最后dp = [5 2 4 1]
        [0 3 1 4]
        [0 0 4 1]
        [0 0 0 5]
'''
def stoneGame():
    n = int(input())#n个石子
    piles = [int(x) for x in input().split()]#输入n个石子
    dp = [[0]*n for _ in range(n)]
    for i in range(n):#dp[i][i]存储当前i的石子数
        dp[i][i] = piles[i]
    for d in range(1,n):#d=1,其实代表，先算两个子的时候
        for j in range(n-d):#有多少组要比较，
        	#比较 j 到 d+j
            dp[j][j+d] = max(piles[j]-dp[j+1][j+d], piles[j+d]-dp[j][j+d-1])
    print(dp[0][-1])
```

<a name="归并排序"></a>

```python
'''归并排序-分治递归的思想,代码为二路归并
选择理由：稳定，一般用于总体无序，但各子项相对有序。

基本思想:将列表分成几部分，两两合并。
时间复杂度：最好-最差-平均都是：O(nlogn)这就很厉害了！
空间复杂度：O(n)

归并排序经典应用：
1. 排序链表：
解题思路:递归归并
经典考察三个知识点：归并排序，找中间节点，合并链表
'''
def MergeSort(li):
    #合并左右子序列函数
    def merge(li, left, mid, right):
        temp = [] #中间数组
        i = left#左段子序列起始
        j = mid + 1#右段子序列起始
        while i<= mid and j <= right:
            if li[i] <= li[j]:#要加等号，否则不稳定。
                temp.append(li[i])
                i += 1
            else:
                temp.append(li[j])
                j += 1
        while i <= mid:
            temp.append(li[i])
            i += 1
        while j <= right:
            temp.append(li[j])
            j += 1
        # !注意这里，不能直接li=temp,他俩大小都不一定一样
        for i in range(left, right+1):
            li[i] = temp[i-left]
    #递归调用归并排序
    def msort(li, left, right):
        if left >= right:
            return
        mid = (left+right)//2
        mSort(li, left, mid)
        mSort(li, mid+1, right)
        merge(li, left, mid, right)
    n = len(li)
    if n <= 1:
        return li
    mSort(li, 0, n-1)
    return li
```

<a name="快速排序"></a>

```python
'''快速排序-20世纪十大算法之一，别问我其他九大算法。
选择理由：不稳定，但是后序研究丰富，很多优化方案。进行优化后，该排序算法依旧很强！
优化1：三数取中，即不再将最左边的值当做中间枢纽。而是取左中右三个值。
优化2：对于大数据是效果较好，但是如果数据本身就很小，反而更费事了，因此采用分区大小调整，当数据集
较小时，不必继续递归调用快速排序算法，而改为调用其他的对于小规模数据集处理能力较强的排序算法来完成。
一般使用插入排序，因为当区间较小时，已经几乎完成排序，有着接近线性的复杂度
优化4：数据较小更换排序算法

基本思想:选一个数作为中间枢纽，进行列表的划分，递归。
时间复杂度：最差O(n2)，最好O(nlogn)，平均O(nlogn) -由分区的好坏决定。
空间复杂度：O(n)，主要为递归造成的空间栈的使用
'''
def QuickSort(li):
    def partition(li, left, right):
        key = left#优化1：三数取中，随机选择一个等都会达到O(nlogn)
        while left < right:
            #先从后面找，忽略相等的元素。最后相遇一定是在小元素。
            while left < right and li[right] >= li[key]:
                right -= 1
            while left < right and li[left] <= li[key]:
                left += 1
            li[left],li[right] = li[right],li[left]
              #优化2：直接进行交换，最后再插入中间枢纽:。
        li[key],li[left] = li[left],li[key]
        return left

    def quicksort(li, left, right):#递归调用
        if left >= right:
            return
        mid = partition(li, left, right)
        quicksort(li, left, mid-1)
        #优化3：减少递归栈空间的使用，使用迭代
		#while循环加上left = mid + 1
        quicksort(li, mid+1, right)
    #主函数
    n = len(li)
    if n <= 1:
        return li
    quicksort(li, 0, n-1)
    return li


```

<a name="计数排序"></a>

```python
'''计数排序-牺牲空间复杂度来使时间复杂度达到线性增长，唯一不使用比较的排序算法。
时间复杂度为 O(n) 有点意思哦！
空间复杂度为 O(n)

对于小规模排序计数排序的时间复杂度和空间复杂度都是效率较高的，但是计数排序对输入有限制，并不是所有情况下都能使用这种排序算法。计数排序的一个大前提就是序列中的每个数字都必须为0到k的整数。

计数排序经典应用：
1. 最大间距
应用：给500万考生成绩排序，超级快，适用于元素集合不大的情况，分数最多是0-1000。
应用：在一个文件中有 10G 个整数，乱序排列，要求找出中位数，内存限制为 2G，只写出思路即可。
'''
def CountAlgorithm(org_li, max_num):
    temp_li = [0]*(max_num+1)
    sorted_li = [0]*len(org_li)
    #统计原始列表中不同数字的个数，以数字为下标，在新列表中统计
    for i in range(len(org_li)):
        temp_li[org_li[i]] += 1
    print(temp_li)

    #统计小于当前坐标的数字有多少个,迭代方式   
    for i in range(1, max_num+1):
        temp_li[i] += temp_li[i-1] 
    print(temp_li)

    #找到某个数的个数减一，就是他在新列表的坐标。
    for x in org_li[::-1]:#如果要让它是稳定的，需要逆序输出
        temp_li[x] -= 1
        sorted_li[temp_li[x]] = x
    return sorted_li
org_li=[2,5,3,0,2,3,0,3]
max_num = 5
print(CountAlgorithm(org_li,max_num))
```

  <a name="桶排序"></a>

```python
'''桶排序：工作的原理是将数组分到有限数量的桶子里。每个桶子再个别排序
（有可能再使用别的排序算法或是以递归方式继续使用桶排序进行排序）。
桶排序是鸽巢排序的一种归纳结果。当要被排序的数组内的数值是均匀分配的时候，
桶排序使用线性时间（Θ（n））。但桶排序并不是比较排序，他不受到 O(n log n)下限的影响。
'''
def bucketSort(nums):
#选择一个最大的数
  max_num = max(nums)
#创建一个元素全是0的列表, 当做桶
  bucket = [0]*(max_num+1)
#把所有元素放入桶中, 即把对应元素个数加一
  for i in nums:
    bucket[i] += 1
#存储排序好的元素
  sort_nums = []
#取出桶中的元素
  for j in range(len(bucket)):
    if bucket[j] != 0:
      for y in range(bucket[j]):
        sort_nums.append(j)
  return sort_nums
nums = [5,6,3,2,1,65,2,0,8,0]
print(bucketSort(nums)) 
```



**下面是一些经典排序的算法**

  <a name="乱数排序"></a>

```python
'''洗扑克牌-乱数排序
思路：如何有效打乱一个数组：遍历数组的位置，然后产生随机数，将随机数位置与当前位置交换即可。
输入：poker = [1,...,52]
'''
import random
def washPoker(poker):
    for i in range(52):
        j = random.randint(0,51)
        poker[i],poker[j] = poker[j],poker[i]
    return poker
```

  <a name="三色旗"></a>

```python
'''三色旗排序法
题目概述：将蓝白红三种旗子，按照顺序排好，要求移动次数最少，一次只能调换两个旗子。
解题思路：设置三个指标，blue = white = 0，red = len-1,然后从左往右判断，如果旗子颜色是，白色：w++，蓝色： swap(b++,w++)，红色： r 向左移动到非红色，再 swap(w,r)
'''
def AlgorithmGossip(flags):
    b,w,r = 0,0,len(flags)-1
    while w <= r:
        if flags[w] == 'w':
            w += 1
        elif flags[w] == 'b':
            flags[b],flags[w]=flags[w],flags[b]
            b += 1
            w += 1
        else:
            while w < r and flags[r] == 'r':
                r -= 1
            flags[w],flags[r] = flags[r],flags[w]
            r -= 1
    return flags
```

  <a name="摆动排序"></a>

```python
'''摆动排序
题目概述：将数组排序成a <= b >= c <= d >= e......
解题思路1: 复杂度O(nlogn)，先排序，再调换二三，四五，...等的位置
half = (len(nums)+1)//2
nums.sort()
nums[::2],nums[1::2] = nums[:half][::-1],nums[half:][::-1]

解题思路2: 复杂度O(n)，分析可知，当i为奇数时，nums[i] >= nums[i - 1]
当i为偶数时，nums[i] <= nums[i - 1]
'''
def One(nums):
    nums.sort()
    if len(nums) <= 2:
        return 
    for i in range(2, len(nums), 2):
        nums[i],nums[i-1] = nums[i-1],nums[i]

def Two(nums):#不能有相同的值，否则会出错
    if len(nums) <= 1:
        return
    for i in range(1, len(nums)):
        if (i%2==1 and nums[i] < nums[i-1]) || (i%2==0 and nums[i] > nums[i-1]):
            nums[i],nums[i-1] = nums[i-1],numss[i]
```

  <a name="煎饼排序"></a>

```python
'''煎饼排序
题目概述：给定数组 A=[1,...,n] 不重复打乱顺序的连续值，我们可以对其进行煎饼翻转：我们选择一些正整数 k <= A.length，然后反转 A 的前 k 个元素的顺序。我们要执行零次或多次煎饼翻转（按顺序一次接一次地进行）以完成对数组 A 的排序。返回能使 A 排序的煎饼翻转操作所对应的 k 值序列。任何将数组排序且翻转次数在 10 * A.length 范围内的有效答案都将被判断为正确。来源：力扣（LeetCode）
解题思路:倒序排，将 A 中的元素从大到小依次反转到数组的尾部。

'''
def nreverse(A, p):#将前 p 个元素反转。
    A[:p] = reversed(A[:p])
def pancakeSort(A):
    tail,res = len(A),[]
    while tail > 1:
        p = A.index(tail)#当前乱序的最大值的坐标
        if p != 0:
            num  = p + 1
            self.nreverse(A, num)
            res.append(num)
        self.nreverse(A, tail)
        res.append(tail)
        tail -= 1
    return res
```

  <a name="距离相等的条形码"></a>

```python
'''距离相等的条形码
题目概述：数组中相同的数字不能相连。[1,1,1,2,2,2]
解题思路:按照数量多少先进行排序，然后按奇偶填充。
'''
def rearrangeBarcodes(barcodes):
    counter = dict(collections.Counter(barcodes))
    #按出现次数统计元素
    sortedCounter = sorted( counter, key=lambda k: 0 - counter[k])
    barcodes = []
    #重新排序
    for i in sortedCounter:
        barcodes += [i] * counter[i]
        arrangedBarcodes = [None for _ in range(len(barcodes))]
#间隔插入
arrangedBarcodes[::2] = barcodes[:len(arrangedBarcodes[::2])]
arrangedBarcodes[1::2] = barcodes[len(arrangedBarcodes[::2]):]

return arrangedBarcodes
```

  <a name="距离顺序排列矩阵单元格"></a>

```python
'''距离顺序排列矩阵单元格
题目概述：给定矩阵中的一个坐标，按曼哈顿距离排列矩阵中其余的坐标。
曼哈顿距离：|r1 - r2| + |c1 - c2|
解题思路：利用空间换时间的想法，构造距离空间。
'''
def allCellsDistOrder(R, C, r0, c0):
    ans = []
    dic = {}
    for i in range(R):
        for j in range(C):
            diff = abs(i-r0)+abs(j-c0)
            print(diff)
            if diff not in dic:
                dic[diff] = [[i,j]]
            else:
                dic[diff].append([i,j])
                for i in range(R+C):
    if i in dic.keys():
        ans.extend(dic[i])
return ans
```





