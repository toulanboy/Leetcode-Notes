## 题目

<<https://leetcode.com/problems/zigzag-conversion/>

## 题意

给出一个序列，按照某种规则重新放置序列的字符，然后逐行输出结果。

例子：`"PAYPALISHIRING"` 的放置结果是：

```
P   A   H   N
A P L S I I G
Y   I   R
```

**Example 1:**

```
Input: s = "PAYPALISHIRING", numRows = 3
Output: "PAHNAPLSIIGYIR"
```

**Example 2:**

```
Input: s = "PAYPALISHIRING", numRows = 4
Output: "PINALSIGYAHRPI"

Explanation:

P     I    N
A   L S  I G
Y A   H R
P     I
```

## 思路

这道题比较简单，其实就是一道找简单规律的题目。

我将竖着的字母和斜着的字母看着是一组。例如Example2可以看成：

```
P     I    N
A     S    G
Y     H 
P     I
A     R
L     I
```

直接对字符位置下标取余，就可以知道该字符在组中的位置。

但是一共只有rowNum行，所以对于组中的前rowNum行，直接放置。超出rowNum行的，从倒数第rowNum-1行开始放置。

## 首次实现的代码A

```c++
class Solution {
public://author's blog == http://www.cnblogs.com/toulanboy/
    string convert(string s, int numRows) {
        int groupNum = numRows + numRows - 2;
        string result = "";
        if(s.length() == 0)
            return result;
        if(numRows == 1)
            return s;
        queue<char> * rowsChar = new queue<char>[groupNum];
        for(int i=0; i<s.length(); ++i){
            int pos = i%groupNum;
            if(pos < numRows)
                rowsChar[pos].push(s[i]);
            else
                rowsChar[2*numRows-2-pos].push(s[i]);
        }
        for(int i=0; i<numRows; ++i){
            while(!rowsChar[i].empty()){
                result = result + rowsChar[i].front();
                rowsChar[i].pop();
            }
        }
        delete [] rowsChar;
        return result;
    }
};
```


## 代码A运行结果

```
Runtime: 100 ms
Memory Usage: 194.1 MB
```

## 可以优化的点

不需要用队列。直接根据位置关系放到结果字符串中。省了时间，还省空间~

## 最终代码

```c++
class Solution {
public://author's blog == http://www.cnblogs.com/toulanboy/
    string convert(string s, int numRows) {

        if (numRows == 1) return s;

        string result;
        int len = s.length();
        int groupNum = 2 * numRows - 2;

        for (int i = 0; i < numRows; i++) {
            for (int j = 0; j + i < len; j += groupNum) {
                result += s[j + i];
                if (i != 0 && i != numRows - 1 && j + groupNum - i < len)
                    result += s[j + groupNum - i];
            }
        }
        return result;
    }
};
```

## 最终代码运行结果

```
Runtime: 12 ms, faster than 98.18% of C++ online submissions for ZigZag Conversion.
Memory Usage: 10.2 MB, less than 87.38% of C++ online submissions for ZigZag Conversion.
```

