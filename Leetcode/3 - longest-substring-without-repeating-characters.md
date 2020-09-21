## 题目

<<https://leetcode.com/problems/longest-substring-without-repeating-characters>>

## 题意

给出一个字符串，求每个字符都不相同的最长子串长度。

现在题目要求我们求出这两个数的和。

**Example 1:**

```
Input: "abcabcbb"
Output: 3 
Explanation: The answer is "abc", with the length of 3. 
```

## 思路

### 错误的思路

**二分 + Hash**

1、既然要找每个字符都不相同的最长子串长度，那么便可以使用二分思想。首先取中值，假如当前中值是存在每个字符都不相同的子串的，说明应该搜索右边的区间。否则，搜索左边区间。这个二分答案的时间复杂度大概是O(log n)。

2、这里还涉及如何判断是否存在长度为K的不相同子串。我的想法是，拿出每一个子串，然后对每个子串使用Hash思想，对字符出现次数进行统计，若有出现2次的字符，那么说明该子串不成立。若遇到一个全部只出现1次的，那么就终止搜索。这一步的时间复杂度是O(n^2)

**很可惜，一共900多个样例，最后一个超时了！**

### 正确的思路

**滑动窗口**

看了题解，才发现大佬的做法真牛P。他的时间复杂度是O(n)。

大概思路是：维护一个字符串不相同的区间，下面把它称为一个窗口。这个窗口只包含互不相同的连续字符。

具体的做法是：设定该区间的左右区间分别是i，j。然后从左到右遍历字符串，对于每一个新来的字符，先判断该字符是否在区间内出现了。

（1）若出现了，说明`区间+新字符`形成的`新字符串`是存在重复字符的。那么不应该将字符加入该区间（窗口不应该向右扩增），而且应该将区间的左边界收缩（窗口左边往右收缩），直到对于新字符来说，区间内不存在重复字符。`（该区间要时刻保证，只包含互不相同的连续字符）`

（2）若没在区间内出现过，说明当前存在更长的不相同的子串。故将该字符加入区间（窗口向右扩增），并且更新最大值。

### 正确的代码

>  该代码是参考题解写的。

```c++
class Solution {
public:

    int lengthOfLongestSubstring(string s) {
        int a[256];
        int n = s.length();
        memset(a, 0, sizeof(int)*256);
        int i = 0;//“不相同子串”的左区间
        int j = 0;//“不相同子串”的右区间
        int ans = 0;
        while(i<n && j<n){
            //如果新的字符没有在“不相同子串”中出现，说明有更大的不相同子串。
            if(a[s[j]] == 0){
                a[s[j]] = 1;
                j++;
                ans = max(ans, j-i);
            }
            else{
                a[s[i]] = 0;
                i++;
            }
            
        }
        return ans;
        
    }
};

```

### 错误的代码

>  该代码是辣鸡我写的，有1个样例超时了。

```c++
class Solution {//toulanboy
public:

    int lengthOfLongestSubstring(string s) {
 
        
        long long left = 0;
        int right = s.length();
        long long a[300];
        while(left < right){
            int mid = ceil((left + right)/2.0);

            bool isExist = false;

            //判断是否存在长度为mid的互不相同的子串
            for(int i=0; i+mid-1<s.length();++i){
                if(isExist){
                    break;
                }
                memset(a, 0, sizeof(long long)*300);
                for(int j=0; j<mid; ++j){
                    a[s[i+j]]++;
                }
                int sum = 0;
                for(int j=0; j<300; ++j){
                    if(a[j] == 1)
                        sum += 1;
                }
                if(sum == mid)
                    isExist = true;


            }

            if(isExist){
                left = mid;
            
            }
            else{
                right = mid - 1;
            }
        }
        return left;
    }
};
```

## 运行结果

```
Runtime: 12 ms, faster than 99.84% of C++ online submissions for Longest Substring Without Repeating Characters.
Memory Usage: 9.2 MB, less than 99.68% of C++ online submissions for Longest Substring Without Repeating Characters.
```

