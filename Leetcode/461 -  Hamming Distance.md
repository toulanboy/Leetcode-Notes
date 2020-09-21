## 前言

刚才在看汉明距离，刚好那篇博文上附上了相关题目，刚好又是leetcode的，所以做了一下。

两个字符串的汉明距离可以理解成：两个字符对应位字符不相同的数目。

所以实现起来其实超简单~所以这道是--水题。


## 题目

<https://leetcode.com/problems/hamming-distance/>

## 题意

给出两个整数，计算这两个整数的汉明距离。

（两个整数的汉明距离是整数二进制的对应位不相同的数目）

**Note:**
0 ≤ `x`, `y` < 2<sup>31</sup>.

**Example 1:**

```
Input: x = 1, y = 4

Output: 2

Explanation:
1   (0 0 0 1)
4   (0 1 0 0)
       ↑   ↑

The above arrows point to positions where the corresponding bits are different.
```



## 思路

直接用除二取整法得到对应的二进制。再进行二进制对比。

唯一需要注意的地方是：两个整数大小不一致，对应的二进制位数不一样。

一般做法可能是将短的补长，但是这样比较麻烦。

目前我对位数长短比较问题的处理方案是：

（1）假设x的二进制长度是a，y的二进制长度是y。

（2）先保证小的数肯定在x，所以x对应的二进制串肯定是最短的。

（3）在两两比较时，先执行a个位置的对比（x二进制的第1~a位，和y二进制的第b-a+1~b位进行比较），再执行y多出的位数与0进行比较。

## 代码

```c++
# include<stack>
class Solution {
public:
    char * getBinary(int key){
        char * binaNum = new char[33];
        stack<int>result;
        while(key){
            result.push(key%2);
            key /= 2;
        }
        int k=0;
        while(!result.empty()){
            binaNum[k++] = result.top() + '0';
            result.pop();
            
        }
        binaNum[k++] = '\0';
        return binaNum;
    }
    int hammingDistance(int x, int y) {
        if(x > y){
            int t = x;
            x = y;
            y = t;
            
        }
           
        char * binaNum_1 = getBinary(x);
        char * binaNum_2 = getBinary(y);
        int len1 = strlen(binaNum_1);
        int len2 = strlen(binaNum_2);
        int gap = len2-len1;
        
        int i, diff = 0;
        for(i=0; i<len1; ++i){
            if(binaNum_1[i] != binaNum_2[i+gap])
                diff++;
        }
        for(i=0; i<gap; ++i){
            if(binaNum_2[i] != '0')
                diff++;
        }
        return diff;
    }
};
```


## 运行结果

```
Runtime: 4 ms, faster than 97.50% of C++ online submissions for Hamming Distance.
Memory Usage: 8.7 MB, less than 99.64% of C++ online submissions for Hamming Distance.
```

