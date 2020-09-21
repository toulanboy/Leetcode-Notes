## 题目

<https://leetcode.com/problems/swap-nodes-in-pairs/>

## 题意

给出一个链表。要求两两交换第i个位置和第i+1个位置的节点。（若下标从0开始用，则i是偶数）

**Example:**

```
Given 1->2->3->4, you should return the list as 2->1->4->3.
```

## 思路

>  author's blog == http://www.cnblogs.com/toulanboy/

这道题也是一道考核链表熟悉度的题目。

（1）其实思路很简单，只需用两个指针，分别指向2个相邻节点，然后实现节点交换即可。

（2）由于需要进行节点交换，所以需要知道两个节点的前驱。（代码中我写成父亲节点了）。

（3）需要注意的是，Leetcode的链表的第一个元素就是有效值，所以第1个元素和第2个元素的交换需要特殊处理。`因为第1个元素没有前驱。`

## 代码

```c++
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
    //author's blog == http://www.cnblogs.com/toulanboy/
    ListNode* swapPairs(ListNode* head) {
        //如果链表只有0个数或者1个数，直接返回原链表
        if(head == NULL || head->next == NULL)
            return head;
        
        //先特殊处理一下头结点和第2个节点
        //将第1个点插在第2个点后，实现交换
        ListNode* tmp = head->next->next;
        head->next->next = head;
        head = head->next;
        head->next->next = tmp;
        
        //开始循环处理后面的节点
        ListNode* one_father = head->next; //AB交换，这是A节点的父亲，刚开始指向第3个点
        ListNode* two_father = NULL;        //AB交换，这是B节点的父亲
        
        //如果A节点存在，并且该节点有对应的B节点
        while(one_father->next != NULL && one_father->next->next != NULL){
            two_father = one_father->next;
            
            
            //为了更容易理解，在这里再用指针指向A和B
            ListNode * one = one_father->next;
            ListNode * two = two_father->next;
            
            //更换指针，实现交换          
            one_father->next = two;
            tmp = two->next;
            two->next = one;
            one->next = tmp;
            
            //继续下一组
            one_father = one_father->next->next;
        }
        return head;
    }
    //author's blog == http://www.cnblogs.com/toulanboy/
};
```


## 运行结果

```
Runtime: 4 ms, faster than 94.43% of C++ online submissions for Swap Nodes in Pairs.
Memory Usage: 8.6 MB, less than 72.88% of C++ online submissions for Swap Nodes in Pairs.
```

