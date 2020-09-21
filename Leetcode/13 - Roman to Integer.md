## 题目

<https://leetcode.com/problems/roman-to-integer/>

## 题意

给出一个罗马数字，要求你输出对应的整数



## 思路

>  author's blog == http://www.cnblogs.com/toulanboy/

 这道题就比整数转罗马简单多了。

（1）总体思路是：

从前往后扫一遍罗马串，如果遇到一个小罗马后面跟着一个大罗马，则result加上大罗马减去小罗马的数值。否则直接加上小罗马的数值。

（2）优化点：

对比罗马字符大小时，由于罗马字符对应的大小和ASCII并没有关系，所以导致不能比较罗马字符得到大小关系。但我们知道一个罗马字符对应一个数值，罗马字符的对比实际上是数值的对比。然而，如果每次都需要一个遍历才能确定一个罗马字符的数值，那么就很麻烦了。所以我们可以用Hash思想，讲第i个罗马字符映射到int数组的第i个位置，并将该字符对应的数值存储在第i个位置。这样就可以直接查找该数组的第i个位置得到对应数值了。



## 代码

```c++
//author's blog == http://www.cnblogs.com/toulanboy/
class Solution {
public:
    int romanToInt(string s) {
        int result = 0;
        int value[256];
        value['I'] = 1;
        value['V'] = 5;
        value['X'] = 10;
        value['L'] = 50;
        value['C'] = 100;
        value['D'] = 500;
        value['M'] = 1000;
        int i = 0;
        while(i<s.length()){
            if(i+1 < s.length() && value[s[i]] < value[s[i+1]]){
                result += value[s[i+1]] - value[s[i]];
                i+=2;
            }
            else{
                result += value[s[i]];
                i+=1;
            }
        }
        return result;
    }
};
//author's blog == http://www.cnblogs.com/toulanboy/
```


## 运行结果

```
Runtime: 12 ms, faster than 97.87% of C++ online submissions for Roman to Integer.
Memory Usage: 8.5 MB, less than 83.17% of C++ online submissions for Roman to Integer.
```

