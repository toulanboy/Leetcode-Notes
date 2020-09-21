## 题目

<<https://leetcode.com/problems/median-of-two-sorted-arrays>>

## 题意

给出两个有序的数组，找出这两个数组所有数的中位数。

**Example 1:**

```
nums1 = [1, 3]
nums2 = [2]

The median is 2.0
```

**Example 2:**

```
nums1 = [1, 2]
nums2 = [3, 4]

The median is (2 + 3)/2 = 2.5
```

## 思路

**归并排序的思想**

由于两个数组是有序的。那么直接利用归并排序中合并数组的思想，从前往后遍历数组，找到中位数位置的数据即可。

1. 假如数据总数是偶数，那么中位数是中间的2个除以2。
2. 假如数据总数是偶数，那么中位数直接便是中间的那一个。

> 虽然刚开始我确实是这样想的。但是我不确定，因为题目说要O(log(m+n))的复杂度，但是这个思路的复杂度是O(m+n)呀。想了一会后，感觉没有更好的想法了，就看了题解。发现很多人都是和我差不多的想法，而且仍然可以AC。最后我还是用了这种。题解的最优解法没看懂。。 

## 代码

```c++

class Solution {//toulanboy
public:
    double findMedianSortedArrays(vector<int>& nums1, vector<int>& nums2) {
        int n = nums1.size();
        int m = nums2.size();
        
        int sum = n+m;
        int mid = sum/2+1;
        
        int i=0, j=0;
        int k = 0;
        
        //同时记录两个中位数
        int val1 = 0;
        int val2 = 0;
        
        //利用归并排序中合并数组的思想
        //从前往后找第k个数是谁
        while(i < n && j < m){
            k++;
            if(nums1[i] < nums2[j]){
                val1 = val2;
                val2 = nums1[i];
                i++;
            }
            else{
                val1 = val2;
                val2 = nums2[j];
                    j++;
            }
            if(k == mid){
                break;
            }
        }
        
        if(k == mid && sum%2)
            return val2;
        else if(k == mid && !sum%2)
            return (val1 + val2)/2.0;
        //若A数组还有剩
        while(i < n){
            k++;
           
            val1 = val2;
            val2 = nums1[i];
            i++;

            if(k == mid){
                break;
            }
        }
        //若B数组还有剩
        while(j < m){
            k++;
           
            val1 = val2;
            val2 = nums2[j];
            j++;

            if(k == mid){
                break;
            }
        }
        if(sum%2)
            return val2;
        else
            return (val1 + val2)/2.0;
    }
};
```


## 运行结果

```
Runtime: 24 ms, faster than 99.34% of C++ online submissions for Median of Two Sorted Arrays.
Memory Usage: 9.5 MB, less than 100.00% of C++ online submissions for Median of Two Sorted Arrays.
```

