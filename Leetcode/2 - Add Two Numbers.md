### 前言

第1次写Leetcode，把这次做的题思路写一下吧。

### 题目

<https://leetcode.com/problems/add-two-numbers/>

### 题意

有2个链表是用来存两个整数的每一位的，而且是将数字从右往左存在链表中的。

现在题目要求我们求出这两个数的和。

**Example:**

```c++
Input: (2 -> 4 -> 3) + (5 -> 6 -> 4)
Output: 7 -> 0 -> 8
Explanation: 342 + 465 = 807.
```

### 思路

正常的加法便是对数字进行从右往左遍历，逐位相加。而现在链表刚好存的就是从右往左的具体数字。我们直接对应位置相加就行。

只是，对应位置相加可能会出现进位（和大于10的数），故我们需要处理一下。

故总体流程如下：

1. 分别遍历两条链表。进行对应位相加，得出对应位的和。
2. 判断对应和的是否大于10，若大于10，则进位。
   1. 若当前位的下一位仍有数字，那么直接在下一位加1.
   2. 若当前位的下一位仍有数字，则在当前位后面创建一个节点，数值为进上去的1.
3. 若对应位相加后，部分链表仍有数据。则进行以下操作（说明这两个数长度不相同）
   1. 直接将剩余的数据接在结果链表。但是需要注意的是，这时候由于前面有进位处理，可能该链表中会出现大于10的数，所以也需要逐一处理剩余的位数。（原理同上述步骤2）

### 代码

```c++
/**
 * Definition for singly-linked list.//toulanboy
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
class Solution {
public:
    ListNode* addTwoNumbers(ListNode* l1, ListNode* l2) {
        ListNode * p1  = l1;
        ListNode * p2 = l2;
        ListNode * p3 = NULL;
        ListNode * p  = p3;
        int in = 0;
        //遍历对应的位置
        while(p1 && p2){
            int sum = p1->val + p2->val;
            if(sum >= 10){//处理进位
                if(p1->next){//（1）直接放在下一位
                    p1->next->val++;
                }
                else{//（2）下一位不存在，则创建。
                    p1->next = new ListNode(1);
                    if(!p2->next){
                        p2->next = new ListNode(0);
                    }
                }
                //进位后，当前位减10
                sum -= 10;
            }
            if(p3 == NULL){
                p3 = new ListNode(sum);
                p = p3;
            }
            else{
                //将当期位相加的和放在结果链表
                ListNode * pNew = new ListNode(sum);
                p->next = pNew;
                p = pNew;
            }
            p1 = p1->next;
            p2 = p2->next;
            
        }
        //处理剩余的位数
        while(p1){
            if(p1->val >= 10){
                p1->val -= 10;
                if(p1->next){
                    p1->next->val++;
                }
                else{
                    p1->next = new ListNode(1);
                }
            }
            p->next = p1;
            p = p1;
            p1 = p1->next;
        }
        //处理剩余的位数
        while(p2){
            if(p2->val >= 10){
                p2->val -= 10;
                if(p2->next){
                    p2->next->val++;
                }
                else{
                    p2->next = new ListNode(1);
                }
            }
            p->next = p2;
            p = p2;
            p2 = p2->next;
        }
        return p3;
    }
};
```

### 运行结果

```
Runtime: 32 ms, faster than 86.82% of C++ online submissions for Add Two Numbers.
Memory Usage: 10.3 MB, less than 99.82% of C++ online submissions for Add Two Numbers..
```

