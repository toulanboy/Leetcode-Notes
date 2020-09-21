## 题目

<https://leetcode.com/problems/integer-to-roman/>

## 题意

给出一个整数，要求你将其转换成罗马数字。

```
Symbol       Value
I             1
V             5
X             10
L             50
C             100
D             500
M             1000
```

**Example 1:**

```
Input: 3
Output: "III"
```

**Example 2:**

```
Input: 4
Output: "IV"
```

**Example 3:**

```
Input: 9
Output: "IX"
```

**Example 4:**

```
Input: 58
Output: "LVIII"
Explanation: L = 50, V = 5, III = 3.
```

**Example 5:**

```
Input: 1994
Output: "MCMXCIV"
Explanation: M = 1000, CM = 900, XC = 90 and IV = 4.
```

## 思路

>  author's blog == http://www.cnblogs.com/toulanboy/

 （1）这道题其实就是一个数字拼凑，可以直接用深搜来解决。

我们可以把该数字看成是最多有N个罗马数字组成，每个位置罗马数字的选择情况就构成了问题状态空间。该状态空间的层次便是第i个位置选择的罗马数字。我们只需遍历整个状态空间，就可以得到我们要的答案。

（2）而问题是：每个位置的数字的选择有哪些。

通过题目，可知有7个数字表示方式，而且题目也告知我们是可以两个组合使用的，如小的在大的前面，那么数值就是大的减去小的。从这些信息来看，我以为有28种表示方式。然而，题目很坑爹呀，他并没有告诉我们完整规则。实际上，通过查阅知乎[1]和5次的自信含泪提交，最终发现只有13种表示方式。具体表示方式请看下面代码实现。

（3）在深搜过程中可以优化搜索顺序。优先填写大的数字，这样更容易达到指定的和。其实从罗马数字的规则来看，罗马数字也应该用尽量短的字符串来表示的。所以这个剪枝既可以说是使用了技巧，也可以说迎合了题目规则。

（4）这道题假如不优先填写大的数字，那么虽然最终求出的罗马数字也符合数值，但是不是最短的。所以如果不采用（3）中的搜索顺序，那么可以采用`迭代加深`的思想。在迭代加深的过程中，实际上便是不停地延长答案长度的过程，这样可以确保搜索到的答案肯定是最短的。若读者感兴趣，可以自行实现该思路~

## 代码

```c++
//author's blog == http://www.cnblogs.com/toulanboy/
class Solution {
public:
    string roman[13] = {"M", "CM", 
                      "D","CD", 
                      "C", "XC", 
                      "L",  "XL", 
                      "X", "IX", 
                      "V", "IV",
                      "I"};

    int value[13] = {1000, 900,
                   500, 400,
                   100, 90, 
                   50, 40,
                   10, 9, 
                   5, 4,
                   1};


    int caseNum = 13;
    stack<string> result;


//author's blog == http://www.cnblogs.com/toulanboy/
    bool dfs(int sum, int target){
        if(sum == target){

            return true;
        }
        for(int i=0; i<caseNum; ++i){
            if(sum + value[i] <= target){
                result.push(roman[i]);
                if(dfs(sum+value[i], target))
                    return true;
                result.pop();
            }
        }
        return false;
    }
    
    string intToRoman(int num) {
        dfs(0, num);
        stack<string> temp;
        while(result.empty() == false){
            temp.push(result.top());
            result.pop();
        }
        string romanResult = "";
        while(temp.empty() == false){
            romanResult += temp.top();
            temp.pop();
        }
        return romanResult;
    }
};
```


## 运行结果

```
Runtime: 12 ms
Memory Usage: 15.2 MB
```

## 参考文章

【1】 [罗马数字的构造规则](https://zhuanlan.zhihu.com/p/32305410)