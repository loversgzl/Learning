

# Java-算法之动态规划

简介：动态规划一般解最优问题，且解法巧妙，但如果解决不了，则可以使用贪心算法解决最优问题。

1. <a href="#最长上升子序列">最长上升子序列</a>

* **最长上升子序列**  <a name="最长上升子序列"></a>
题目概述：一个数组，求里面的最长递增序列，不要求连续
解题思路：
一、插入排序，不断更新数组里的值，保留最小值，插入可以改进为二分插入，
二、动态规划
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

