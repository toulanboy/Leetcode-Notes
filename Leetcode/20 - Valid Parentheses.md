## 题目

<https://leetcode.com/problems/valid-parentheses/>

## 题意

给出一个括号序列，判断这些括号是否满足一一对应关系。

如：左括号必须和右括号匹配

**Example 1:**

```
Input: "()"
Output: true
```

**Example 2:**

```
Input: "()[]{}"
Output: true
```

**Example 3:**

```
Input: "(]"
Output: false
```

**Example 4:**

```
Input: "([)]"
Output: false
```

**Example 5:**

```
Input: "{[]}"
Output: true
```

## 思路

>  author's blog == http://www.cnblogs.com/toulanboy/

 这是一道括号匹配问题，也是一道经典的栈题目。

首先，对于每一个右括号都需要和一个左括号匹配对应。那么正常来说，我们可以从前往后遍历，每找到一个右括号，就看看他前面有没有左括号与其匹配。下一个右括号和更前的一个左括号匹配。

而上述思路可以用栈帮我们轻松实现。具体解法如下：

（1）创立一个栈，用来存储左括号。

（2）每遇到一个左括号，就将其进栈。

（3）每遇到一个右括号。先看看栈是否为空。

（3.1）如果空了，说明没有左括号与其匹配，说明该序列是错误的。

（3.2）如果栈不空，就判断其与栈顶的左括号是否匹配。如果是，则继续。否则说明该序列错误。

## 代码

```c++
//author's blog == http://www.cnblogs.com/toulanboy/
class Solution {
public:
    bool isValid(string s) {
        stack<char>leftParentheses;
        int i = 0;
        while(i < s.length()){
            if(s[i] == '(' || s[i] == '[' || s[i] == '{')
                leftParentheses.push(s[i]);
            else if(s[i] == ')' || s[i] == ']' || s[i] == '}'){
                if(leftParentheses.empty() == false &&( (leftParentheses.top() == '(' && s[i] == ')') || (leftParentheses.top() == '[' && s[i] == ']') || (leftParentheses.top() == '{' && s[i] == '}'))){
                    leftParentheses.pop();
                }
                else{
                    return false;
                }
            }
            i++;
        }
        return leftParentheses.empty();
    }
};//author's blog == http://www.cnblogs.com/toulanboy/
```


## 运行结果

```
Runtime: 4 ms, faster than 94.49% of C++ online submissions for Valid Parentheses.
Memory Usage: 8.6 MB, less than 59.52% of C++ online submissions for Valid Parentheses.
```

