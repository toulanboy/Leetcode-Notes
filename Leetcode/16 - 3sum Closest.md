## 题目

<https://leetcode.com/problems/3sum-closest/>

## 题意

> 如果对这道题感兴趣，不妨先做下这一道：
>
> 题目：<https://leetcode.com/problems/3sum>
>
> 题解：<https://www.cnblogs.com/toulanboy/p/10900105.html>

给出一个整数序列，对于序列中的任意三个数都会有1个和。求该序列和指定目标最接近的和。

**Example:**

```
Given array nums = [-1, 2, 1, -4], and target = 1.

The sum that is closest to the target is 2. (-1 + 2 + 1 = 2).
```



## 思路

>  author's blog == http://www.cnblogs.com/toulanboy/

 这道题和Leetcode15是一样的思路。只是之前的是求和为0的情况，现在是求和为target的情况。

为了方便读者阅读，下面重复一下解题思路：

>  建议先阅读Leetcode15题解<https://www.cnblogs.com/toulanboy/p/10900105.html

这个思路时间复杂度大概是0(n<sup>2</sup>)。而且和target这个目标是贴合的，利用当前和大小进行移动。具体做法是：

（1）首先升序排序输入序列。

（2） 确定第1个位置的数值。

（3）第2个位置刚开始选择的数值是第1个位置的下一个。第3个位置刚开始选择的数值是序列的最后一个。

（4）判断当前的和。如果等于target，则最近距离肯定是0了，直接返回当前和。如果当前和小于target，说明第2个位置应该选择更大的数值。如果当前和大于target，说明第3个位置的数值应该选取更小的数值。修改后若第2个数位置仍小于第3个数位置，则回到第4步。否则第1个位置选取下一个数值，回到第2步。

## 代码

```c++
//author's blog == http://www.cnblogs.com/toulanboy/
class Solution {
public:
    int threeSumClosest(vector<int>& nums, int target) {

        sort(nums.begin(), nums.end());
        int result = INT_MAX;
        int gap = INT_MAX;
        int i, j, k, sum;
        for(i=0; i<nums.size(); ++i){
            if(i != 0 && nums[i] == nums[i-1])
                continue;
            
            j = i + 1;
            k = nums.size()-1;
           
            while(j<k){
                sum = nums[i] + nums[j] + nums[k];
                if(sum == target){
                    return sum;
                }
                if(abs(sum - target) < gap){
                    gap = abs(sum - target);
                    result = sum;
                }
                if(sum < target){
                    while(j < k && nums[j+1] == nums[j])
                        j++;
                    j++;
                }
                if(sum > target){
                    while(j < k && nums[k-1] == nums[k])
                        k--;
                    k--;
                }
            }
        }
        return result;
    }
};//author's blog == http://www.cnblogs.com/toulanboy/
```


## 运行结果

```
Runtime: 8 ms, faster than 97.49% of C++ online submissions for 3Sum Closest.
Memory Usage: 9 MB, less than 55.54% of C++ online submissions for 3Sum Closest.
```

