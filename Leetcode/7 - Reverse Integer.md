## 题目

<<https://leetcode.com/problems/reverse-integer/>

## 题意

反转一个32位的有符号整数。假如反转后是溢出的，则反转结果是0。

**Example 1:**

```
Input: 123
Output: 321
```

**Example 2:**

```
Input: -123
Output: -321
```

**Example 3:**

```
Input: 120
Output: 21
```

## 思路

超级水的题目。

>  可惜我写不好判断溢出的，看了题解才知道原来溢出判断也很简单~

总体思路：通过取余和除法，从个位开始往高位，逐步拿出每一位数字，并组合成新的数字。

唯一需要注意的是判断溢出，方案是：在给新数字加上新的一位数字前，都判断如果加了这一位是否会溢出即可。

## 代码

```c++
class Solution {
public://author's blog == http://www.cnblogs.com/toulanboy/
    int reverse(int x) {

        int result = 0;
        while(x){
            int last = x % 10;
            if(result > INT_MAX/10 || (result == INT_MAX/10 && last > 7))
                return 0;
            if(result < INT_MIN/10 || (result == INT_MIN/10 && last < -8))
                return 0;
            
            result = result * 10 + last;
            x = x / 10;
        }

        return result;
            
    }
};
```


## 运行结果

```
Runtime: 0 ms
Memory Usage: 8.3 MB
```

