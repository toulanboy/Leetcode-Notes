## 题目

<https://leetcode.com/problems/container-with-most-water/>



## 题意

给定多个二维数据(k, 𝛼[k])，然后从其中选取2个数据形成一个矩形，如选择了(i, 𝛼[i]), (j, 𝛼[j])。那么，底边长度是 j-i，矩形高度是min(𝛼[i], 𝛼[j])。题目要求我们找到这些数据能组成的矩阵面积的最大值。

附一个官方图示：

![question_11.jpg](https://s3-lc-upload.s3.amazonaws.com/uploads/2018/07/17/question_11.jpg)

上述例子𝛼[i]的数据分别是 [1,8,6,2,5,4,8,3,7]。最大面积是蓝色矩形的面积，49。



## 思路

>  author's blog == http://www.cnblogs.com/toulanboy/

>  （1）看到这种题目，一开始想到的可能是穷举每一个矩形，就可以得到这些矩形的最大值。这种解法的时间复杂度是0(n<sup>2</sup>)。
>
> （2）但是，这道题既然被Leetcode标为中等难度的题目，那么解法应该会有更好的。由于我没有思路，所以我看了下官方题解。
>
> （3）在官方题解中，比较好的解法是双指针法。所以我下面主要用自己的语言尝试阐述一下这种解法。

首先，通过题目可知，一个矩形的面积是和底边、高度相关，而且高度取决于较短的那条。所以我们可以先从底边入手，找到底边最大时的矩形面积。然后尝试不停地找比当前面积还要大的矩形。大概做法是变动这个矩形的边，以改变矩形的高度。

具体做法是：

1、 让左指针i代表矩形左边的边，让右指针j代表矩形右边的边。刚开始时，i = 0, j = n-1。

2、 判断i， j这条边谁短。若i短，则往右选择一条新的边，也就是i+1。否则往左选择一条新的边，也就是j-1。

3、 计算新矩形的面积，判断是否比之前的更大。若是，更新矩形最大值。

4、 重复2、3步，直到i >= j。

在这里解释一下为什么上面两个指针是这样移动的。因为在当前矩形的情况下，假如移动长的去得到新矩形，那么这些新矩形的面积肯定是比较小的。因为矩形面积受限于短的那条边，而移动长的，导致高度不变，但是底边变短了，所以每次都应该移动短的，这样才有可能找到更高的边，让新矩形的面积可能更大。

这种双指针法的时间复杂度是0(n)。



## 代码

```c++

class Solution {
public://author's blog == http://www.cnblogs.com/toulanboy/
    int maxArea(vector<int>& height) {
        int len = height.size();
        int i=0, j=len-1;
        int maxarea = (j-i)*min(height[i],height[j]);
        while(i<j){
            maxarea = max(maxarea,  (j-i)*min(height[i],height[j]));
            if(height[i] < height[j])
                i++;
            else
                j--;
        }
        return maxarea;
    }
};
```


## 运行结果

```
Runtime: 20 ms, faster than 95.76% of C++ online submissions for Container With Most Water.
Memory Usage: 9.9 MB, less than 55.58% of C++ online submissions for Container With Most Water.
```

