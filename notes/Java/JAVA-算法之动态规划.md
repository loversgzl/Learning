

# Java-算法之动态规划

简介：动态规划一般解最优问题，且解法巧妙，但如果解决不了，则可以使用贪心算法解决最优问题。

1. <a href="#最长上升子序列">最长上升子序列</a>
2. <a href="#打家劫舍">打家劫舍</a>
3. <a href="#背包问题">背包问题</a>



* **最长上升子序列**  <a name="最长上升子序列"></a>
题目概述：一个数组，求里面的最长递增序列，不要求连续
解题思路：
一、插入排序，不断更新数组里的值，保留最小值，插入可以改进为二分插入。
二、动态规划，更新状态转移方程。
要求：将算法的时间复杂度降低到O(nlogn)
```java
//插入排序+二分插入
import java.util.ArrayList;
class Solution {
//二分查找，寻找第一个比x大的值。
    public void binaryInsert(ArrayList<Integer> dp, int x){
        int left = 0, right = dp.size()-1, mid = 0;
        while(left < right){
            mid = (left+right)/2;
            if(dp.get(mid) < x)
                left = mid + 1;
            else
                right = mid;
        }
        dp.set(left,x);
    }
    public int lengthOfLIS(int[] nums) {
        if(nums == null)
            return 0;
        if(nums.length <= 1)
            return nums.length;
//遍历数组，如果比目前最大值大，肯定加入队列，否则插入到队列中，因为肯定可以替换里面一个比他大的值
        ArrayList<Integer> dp = new ArrayList<>();
        dp.add(nums[0]);
        for(int i=1; i<nums.length; i++){
            if(nums[i] > dp.get(dp.size()-1))
                dp.add(nums[i]);
            else
                binaryInsert(dp,nums[i]);                
        }
        return dp.size();
    }
}
    
//动态规划
class Solution {
    public int lengthOfLIS(int[] nums) {
        if(nums == null)
            return 0;
        if(nums.length <= 1)
            return nums.length;
        int[] dp = new int[nums.length];
        for(int i=0; i<nums.length; i++)
            dp[i] = 1;
        for(int i=1; i<nums.length; i++){
            for(int j=0; j<i; j++){
//如果新扩展的i能够加入到前面的递增数列中，则更新，遍历所有递增数列，
//如果不能则不更新，所以最大值不一定存在最后一个
                if(nums[j] < nums[i])
                    dp[i] = Math.max(dp[j]+1, dp[i]);
            }
        }
        int max = 0;
        for(int x : dp)
            if(x > max)
                max = x;
        return max;
    }
}
```

* **打家劫舍**
题目概述：给定一个数组，不能选取两个相邻的家庭，计算所能偷到的最大值
解题思路：只有一户，f(1)=A1，两户f(2)=max(A1,A2),三户（第三户不抢，保留最大值，），f(k)=max(f(k-2)+AK,f(k-1))
```java
public int rob(int[] nums) {
    if(nums == null) return 0;
    int prevMax = 0;
    int currMax = 0;
    for(int x : nums){
        int temp = currMax;
        currMax = Math.max(prevMax+x, currMax);
        prevMax = temp;
    }
    return currMax;
}
```

**背包问题**<a name="背包问题"></a>
题目概述：背包容量 w， n 袋零食，第 i 袋零食体积为 v[i]
问：牛牛想知道在总体积不超过背包容量的情况下,他一共有多少种零食放法(总体积为0也算一种放法)。如1、2、4，共有 2*2*2=8 种方法。

```java
/*这一类背包问题有两种方法，一种是当物品个数 n 较少，但是背包大小 m 比较大时采用指数级的枚举搜索，复杂度为O(2^n)，另一种是当背包比较小的时候采用动态规划O(nm)。
这道题明显是属于第一种。但是 2^30 的复杂度是不可以接受的，因此我们可以采用中途相遇法。把我们能接受的前15个物品先进行第一次枚举搜索，然后再对剩下的物品进行第二次枚举搜索。把第二次枚举搜索出来的结果(至多2^15=32768个答案)存入数组并排序，枚举第一次搜出来的结果，计算出还剩下多少背包体积还能装，在第二次的结果中进行二分搜索，并把两次搜索的结果进行相乘(乘法原理)。再把所有的结果进行相加(加法原理)，就是答案了。
我为了让小数据算的快一些，当n<20时直接枚举搜索出答案了。
当然也可以把物品分成n/2和n-n/2两部分。
总复杂度是O(2^(n/2)*n)
*/
```