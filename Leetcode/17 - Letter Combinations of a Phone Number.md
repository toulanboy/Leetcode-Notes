## 题目

<https://leetcode.com/problems/letter-combinations-of-a-phone-number/>

## 题意

已知在九宫格的输入法上，2对应的字符是a\b\c，3对应的字符是def，以此类推。

现在让你在九宫格上按某个整数，求输出该整数可能产生的所有字符串。

**Example:**

```
Input: "23"
Output: ["ad", "ae", "af", "bd", "be", "bf", "cd", "ce", "cf"].
```

## 思路

>  author's blog == http://www.cnblogs.com/toulanboy/

首先用一个数组先记录好0-9对应的字符串。

然后DFS得到所有可能的字符串。对于这个问题来说，状态空间的层次是每一个位置应该填的数。而第i个位置能填的数值则由整数中的第i位来决定。

## 代码

```c++
//author's blog == http://www.cnblogs.com/toulanboy/
class Solution {
public:
    vector<string> result;
    //current是当前字符串，depth是当前深度，也就是当前函数决定第depth个位置填啥
    void dfs(string current, int depth, string digits, string * numValue){
        if(depth == digits.length()){
            //当深度和数字长度相等，说明前面的n个数字都转换好了
            result.push_back(current);
            return;
        }
        int num = digits[depth] - '0';
        //枚举该位置能填写的字符
        for(int i=0; i<numValue[num].length(); ++i){
            //填写好了当前位置，去填写下一个位置
            dfs(current + numValue[num][i], depth+1, digits, numValue);
        }
        return;
    }
    
    vector<string> letterCombinations(string digits) {
        //先记录好0-9对应的字符串
        string numValue[10] = {"", "", "abc", "def", "ghi", "jkl", "mno", "pqrs", "tuv", "wxyz"};
        if(digits.length() == 0)
            return result;
        dfs("", 0, digits, numValue);
        return result;
        
    }
};//author's blog == http://www.cnblogs.com/toulanboy/
```


## 运行结果

```
Runtime: 4 ms, faster than 95.71% of C++ online submissions for Letter Combinations of a Phone Number.
Memory Usage: 8.9 MB, less than 22.09% of C++ online submissions for Letter Combinations of a Phone Number.
```