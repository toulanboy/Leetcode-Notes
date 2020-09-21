## 题目

<<https://leetcode.com/problems/longest-palindromic-substring/>

## 题意

给出一个字符串，找出字符串中最长的回文串。

**Example 1:**

```
Input: "babad"
Output: "bab"
Note: "aba" is also a valid answer.
```

**Example 2:**

```
Input: "cbbd"
Output: "bb"
```

## 思路

> 思路源自leecode官方题解。
>
> 下面用我的语言整理总结一下。

这道题可以用动态规划来解决。时间复杂度O(n<sup>2</sup>)。

首先，解决动态规划问题一般分三步。

（1）最优子结构

（2）方程式

（3）边界条件

对于这道题来说，如果“babab”是回文串，那么其"aba"也是回文串。

换句话说，如果str[i~j]是回文串，并且str[i-1] 和str[j+1]相同，那么str[i-1 ~ j+1]也是回文串。说明了这个问题最优的解决方案可以通过最优它的子问题的解决方案来获取，故这个问题有最优子结构的属性。

既然知道存在最优子结构，那么就可以写方程式。

```
首先，我们用p[i][j]表示字符串i到j的子串是否是回文串。
那么，p[i][j] = 1 , if (p[i+1][j-1] = 1 且 s[i] == s[j])
```

最后，我们可以知道长度为1的子串肯定是回文串，若长度为2的子串的前后字符相同，那这个子串也是回文串。故可知边界条件是：

```
p[i][i] = 1;
p[i][i+1] = 1 , if(s[i] == s[i+1])
```



## 代码

```c++
class Solution {
public://author's blog = http://www.cnblogs.com/toulanboy/
    string longestPalindrome(string s) {
        int len = s.length();
        int max_i = 0;
        int max_j = 0;
        int max_p_len = 1;
        int p[1001][1001];
        memset(p, 0, sizeof(int)*1001*1001);
        for(int i=0; i<len; ++i){//处理边界
            p[i][i] = 1;
            if( s[i] == s[i+1]){
                p[i][i+1] = 1;
                if(max_p_len != 2){
                    max_i = i;
                    max_j = i+1;
                    max_p_len = 2;
                }
            }
        }
        for(int i=len-1; i>=0; i--){//p[i][j]依赖于p[i+1][j-1]，所以i从大到小
            for(int j=i+2; j<len; ++j){//只需考虑 j > i 的情况
                if(p[i+1][j-1] == 1 && s[i] == s[j]){
                    p[i][j] = 1;          
                    if(j-i+1 > max_p_len){
                        max_i = i;
                        max_j = j;
                        max_p_len = j-i+1;
                    }
                }
            }
        }
        return s.substr(max_i, max_p_len);//犯错点。记住第二个参数是长度，不是右区间
    }
};
```


## 运行结果

```
Runtime: 68 ms, faster than 55.20% of C++ online submissions for Longest Palindromic Substring.
Memory Usage: 12.8 MB, less than 49.49% of C++ online submissions for Longest Palindromic Substring.
```

## 犯过的错误

记错了substr的用法，以为第二个参数是右区间，实际上是子串的长度。