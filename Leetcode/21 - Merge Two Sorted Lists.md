## 题目

<https://leetcode.com/problems/merge-two-sorted-lists>

## 题意

合并两个有序链表，并且合并结果的链表也是有序的。

**Example:**

```
Input: 1->2->4, 1->3->4
Output: 1->1->2->3->4->4
```

## 思路

>  author's blog == http://www.cnblogs.com/toulanboy/

使用合并排序中的合并思想。

具体做法是：分别用i，j指向两个已排序的链表首个元素。第i个元素和第j个元素比较，谁小谁去新链表。

在我的做法中，我并没有创建新的节点，新链表中的节点都是来自输入数据的2条链表。

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
    ListNode* mergeTwoLists(ListNode* l1, ListNode* l2) {
        if(l1 == NULL)
            return l2;
        if(l2 == NULL)
            return l1;
        
        ListNode * l3 = NULL;
        //先处理第1个节点
        if(l1->val <= l2->val){
            l3 = l1;
            l1 = l1->next;
        }
        else{
            l3 = l2;
            l2 = l2->next;
        }
        //让p指向l3的最后一个节点
        ListNode * p = l3;
        
        while(l1 && l2){
            if(l1->val <= l2->val){
                p->next = l1;
                p = l1;
                l1 = l1->next;
            }
            else{
                p->next = l2;
                p = l2;
                l2 = l2->next;
            }
        }
        if(l1){
            p->next = l1;
        }
        if(l2){
            p->next = l2;
        }
        return l3;
    }
};//author's blog == http://www.cnblogs.com/toulanboy/
```


## 运行结果

```
Runtime: 8 ms, faster than 96.79% of C++ online submissions for Merge Two Sorted Lists.
Memory Usage: 9 MB, less than 65.00% of C++ online submissions for Merge Two Sorted Lists.
```

