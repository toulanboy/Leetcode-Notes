## 题目

<https://leetcode.com/problems/remove-nth-node-from-end-of-list/>

## 题意

给出一个链表，要求删掉倒数第n个位置的数值。

**Example:**

```
Given linked list: 1->2->3->4->5, and n = 2.

After removing the second node from the end, the linked list becomes 1->2->3->5.
```

## 思路

>  author's blog == http://www.cnblogs.com/toulanboy/

 先用一个循环统计一下链表的长度len。

那么题目要求删倒数第n个数值，也就是删掉顺数第len-n+1个数值。

然后用循环找到该数值的前一个，进行指针修改即可完成删除。

唯一需要注意的是：若要删除第一个节点，需要特别处理。

## 代码

```c++
//author's blog == http://www.cnblogs.com/toulanboy/
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
class Solution {
public:
    ListNode* removeNthFromEnd(ListNode* head, int n) {
        ListNode * p = head;
        int num = 0;
        while(p){
            num++;
            p = p->next;
        }
        int pos = num - n + 1;
        
        if(pos == 1){
            return head->next;
        }
        p = head;
        int i=0;
        while(p != NULL){
            i++;
            if(i == pos-1)
                break;
            p = p->next;
        }
        p->next = p->next->next;
        return head;
    }
};//author's blog == http://www.cnblogs.com/toulanboy/
```


## 运行结果

```
Runtime: 4 ms, faster than 98.38% of C++ online submissions for Remove Nth Node From End of List.
Memory Usage: 8.7 MB, less than 58.49% of C++ online submissions for Remove Nth Node From End of List.
```

