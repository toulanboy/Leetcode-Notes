## 题目

<https://leetcode.com/problems/palindrome-number/>

## 题意

判断一个整数是否是回文数。

**Example 1:**

```
Input: 121
Output: true
```

**Example 2:**

```
Input: -121
Output: false
Explanation: From left to right, it reads -121. From right to left, it becomes 121-. Therefore it is not a palindrome.
```

**Example 3:**

```
Input: 10
Output: false
Explanation: Reads 01 from right to left. Therefore it is not a palindrome.
```

## 思路

>  author's blog == http://www.cnblogs.com/toulanboy/

 还是一道超级水题。只需把这个数字反转，然后将反转后的数字和原数字对比。如果一致，说明是回文数。

需要注意的地方是：在反转过程中，用int可能会溢出，用long long存储反转数字即可解决。

## 代码

```c++
class Solution {
public://author's blog == http://www.cnblogs.com/toulanboy/
    bool isPalindrome(int x) {
        if(x < 0)
            return false;
        long long reverseInt = 0;
        int t = x;
        while(t){
            reverseInt = reverseInt*10 + t%10;
            t /= 10;
        }
        if(x == reverseInt)
            return true;
        else
            return false;
    }
};
```


## 运行结果

```
Runtime: 16 ms, faster than 96.66% of C++ online submissions for Palindrome Number.
Memory Usage: 8.1 MB, less than 80.92% of C++ online submissions for Palindrome Number.
```

