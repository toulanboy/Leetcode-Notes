## 题目

<https://leetcode.com/problems/generate-parentheses/>

## 题意

给出左括号的数量n。要求你生成所有的正常的左右括号序列。（每一个右括号都有左括号匹配）

For example, given *n* = 3, a solution set is:

```
[
  "((()))",
  "(()())",
  "(())()",
  "()(())",
  "()()()"
]
```



## 思路

>  author's blog == http://www.cnblogs.com/toulanboy/

（1）这道题可以用搜索`DFS`完成。具体思路如下：

- 这个问题的状态空间的层次是第i个位置的选择情况。每个位置都可以选择左括号活着右括号。由于一共有2*n个位置，那么一共有2\*n层，所以实际上一共有2<sup>n</sup>个状态。

- 当遍历到叶子时，也就是当前2\*n个位置都已经确定好括号，再用0(n)的复杂度验证是否正确。如果符合要求，则加入结果序列。

> 然而，上述做法虽然可行，但是时间复杂度非常高，时间复杂度是0(n * 2<sup>2n</sup>)

（2）考虑剪枝策略，来加速搜索。具体剪枝思路如下：

- 在生成序列的过程中，假如是正常序列，左括号的数量肯定小于n。假如当前左括号大于等于n，说明该位置不用再考虑左括号的填充，故可以把该分支剪掉。

- 在生成序列的过程中，假如是正常序列，右括号的数量肯定小于等于左括号。假如当前右括号大于等于左括号，说明该位置不用再考虑右括号的填充，故可以把该分支剪掉。

>  通过上述剪枝策略，不但可以减去分支，还可以使生成的序列一定满足要求，故也省去了验证序列的步骤。最终时间复杂度是 $ O(\dfrac{4^n}{\sqrt{n}}) $



## 代码

```c++
//author's blog == http://www.cnblogs.com/toulanboy/
class Solution {
public:
    vector<string> result;
    
    void dfs(string ans, int left, int right, int n){
        //如果当前左右括号数等于2*n，说明找到一种case
        if(left + right == 2*n){
            result.push_back(ans);
            return;
        }
        //如果左括号数没达到上限，则在该位置尝试左括号
        if(left < n)
            dfs(ans + "(", left+1, right, n);
        //如果右括号数仍小于左括号数，说明可以在该位置尝试右括号
        if(right < left)
            dfs(ans + ")", left, right+1, n);
        return;
        
    }
    
    vector<string> generateParenthesis(int n) {
        
        string ans = "(";
        int left = 1;
        int right = 0;
        dfs(ans, left, right, n);
        return result;
    }//author's blog == http://www.cnblogs.com/toulanboy/
};
```


## 运行结果

```
Runtime: 4 ms, faster than 97.54% of C++ online submissions for Generate Parentheses.
Memory Usage: 17.6 MB, less than 19.73% of C++ online submissions for Generate Parentheses.
```

