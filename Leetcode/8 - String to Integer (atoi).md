## 题目

<https://leetcode.com/problems/string-to-integer-atoi/submissions/>

## 题意

给出一个字符串，将该字符串转换成int。

若字符串以空格开头，且除空格外第一个字符是“+”、“-”、整数，则正常计算。否则输出0。

若计算过程中，发现是溢出的。则返回32位有符号整数的最大值或最小值。

**Example 1:**

```
Input: "42"
Output: 42
```

**Example 2:**

```
Input: "   -42"
Output: -42
Explanation: The first non-whitespace character is '-', which is the minus sign.
             Then take as many numerical digits as possible, which gets 42.
```

**Example 3:**

```
Input: "4193 with words"
Output: 4193
Explanation: Conversion stops at digit '3' as the next character is not a numerical digit.
```

**Example 4:**

```
Input: "words and 987"
Output: 0
Explanation: The first non-whitespace character is 'w', which is not a numerical 
             digit or a +/- sign. Therefore no valid conversion could be performed.
```

**Example 5:**

```
Input: "-91283472332"
Output: -2147483648
Explanation: The number "-91283472332" is out of the range of a 32-bit signed integer.
             Thefore INT_MIN (−231) is returned.
```

## 思路

> author's blog == http://www.cnblogs.com/toulanboy/

这道题考核的是整数的转换，不包含小数，所以实际上是超级简单的。

唯一需要注意的是题目的限制条件，测试数据中有很多非法字符串。

该题思路简单概况如下：

1、 先判断第一个非空格字符，是否正常。不正常就直接返回0.

2、 判断第一个是否出现了正负号，是的话，做相应正负数标记。

3、 循环组合成新的整数。在拼接新的数字前，先判断溢出情况。

值得注意的是：在我目前的写法中，一直都是以正数处理所有情况。所以正负数溢出判断都是以INT_MAX为基准，不过需要注意INT_MIN的绝对值比INT_MAX大1。

 

## 代码

```c++
class Solution {
public:
    int myAtoi(string str) {//author's blog == http://www.cnblogs.com/toulanboy/
        //先找到第一个非空格字符
        int i=0;
        while(str[i] == ' ')
            i++;
        //若第一个非空格字符不是整数、负号，则返回0
        if(str[i] != '-' &&  str[i] > '9' && str[i] < '0')
            return 0;
        //开始处理
        int result = 0;
        //判断负数
        bool negFlag = false;
        if(str[i] == '-'){
            negFlag = true;
            i++;
        }
        else if(str[i] == '+'){
            negFlag = false;
            i++;
        }
        while(str[i] >= '0' && str[i] <= '9'){
            //加入新的位数前，先判断是否溢出
            int newNum = str[i] - '0';
            if(negFlag == false && (result > INT_MAX / 10 || ( result == INT_MAX / 10 && newNum > 7)))
               return INT_MAX;
            if(negFlag == true  && (result > INT_MAX / 10 || ( result == INT_MAX / 10 && newNum >= 8)))
               return INT_MIN;
            result = result*10 + newNum;
            i++;
        }
        return negFlag?0-result:result;
        
    }
};
```


## 运行结果

```
Runtime: 8 ms, faster than 95.26% of C++ online submissions for String to Integer (atoi).
Memory Usage: 8.4 MB, less than 85.43% of C++ online submissions for String to Integer (atoi).
```

