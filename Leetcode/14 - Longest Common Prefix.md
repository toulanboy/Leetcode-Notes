## 题目

<https://leetcode.com/problems/longest-common-prefix/>

## 题意

给出一组字符串，找出这组字符串的最长公共前缀。

**Example 1:**

```
Input: ["flower","flow","flight"]
Output: "fl"
```

**Example 2:**

```
Input: ["dog","racecar","car"]
Output: ""
Explanation: There is no common prefix among the input strings.
```



## 思路

>  author's blog == http://www.cnblogs.com/toulanboy/

水题。直接对每组字符串的字符进行从前往后的逐一对比即可。

## 代码

```c++
//author's blog == http://www.cnblogs.com/toulanboy/
class Solution {
public:
    string longestCommonPrefix(vector<string>& strs) {
        if(strs.size() == 0)
            return "";
        int i = 0;
        for(i=0; i<strs[0].length(); ++i){
            for(int j=0; j<strs.size(); ++j){
                if(strs[0][i] != strs[j][i])
                    return strs[0].substr(0, i);
            }
        }
        return strs[0].substr(0, i);
    }
};
```


## 运行结果

```
Runtime: 4 ms, faster than 99.42% of C++ online submissions for Longest Common Prefix.
Memory Usage: 8.9 MB, less than 69.83% of C++ online submissions for Longest Common Prefix.
```

