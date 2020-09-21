## 题目

<https://leetcode.com/problems/merge-k-sorted-lists/>

## 题意

给出N条有序的整数链表，要求将其合并成一条有序的整数链表。

## 思路

>  author's blog == http://www.cnblogs.com/toulanboy/

（1）其实是道水题，这道题考核对链表的熟悉运用程度和合并排序的熟悉度。

（2）对于多个有序链表的合并，我们可以用合并排序的思想，每次对比所有链表的首元素，然后取最小的首元素放到结果链表即可。

（3）为了实现上述的循环取数，并且保证所有数值都已经转移到新链表中。我的方案是：每次取完元素后（将该元素从原链表中删除后），我都会判断原链表是否为空；如果空，则将该链表从lists中删除。当lists空了，说明所有数都已经被转移到新链表上了。

> 为了节省空间，在合并过程中，新链表中的节点都是来自原链表。

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
    ListNode* mergeKLists(vector<ListNode*>& lists) {       
        //（1）测试数据有空链表，这我……无语了，那就先预处理一下吧。
        for(int i=0; i<lists.size(); ++i){
            if(lists[i] == NULL){
                lists.erase(lists.begin() + i);
                i--;
            }
        }
        //（2）如果是空的，直接不用处理了，就是NULL
        if(lists.size() == 0)
            return NULL;
        
        //（3）先给结果链表找个头~ 
        ListNode * result = NULL;
        int min_pos = 0;
        for(int i=0; i<lists.size(); ++i)
            if(lists[i]->val < lists[min_pos]->val)
                min_pos = i;
        result = lists[min_pos];
        lists[min_pos] = lists[min_pos]->next;
        //（3.1） 如果该链表去掉该元素后，变成空链表，则将该链表从lists从剔除
        if(lists[min_pos] == NULL)
                lists.erase(lists.begin() + min_pos);
        
        //（4）让p指向最后一个节点，方便在尾部连接新节点
        ListNode * p = result;
        //（5）下面开始用合并排序的思想，合并数列
        // 当lists为空时，说明所有链表都被合并到新链表了
        while(lists.empty() == false){
            min_pos = 0;
            for(int i=0; i<lists.size(); ++i)
                if(lists[i]->val < lists[min_pos]->val)
                    min_pos = i;
           
            p->next = lists[min_pos];
            p = p->next;
            lists[min_pos] = lists[min_pos]->next;
            if(lists[min_pos] == NULL)
                lists.erase(lists.begin() + min_pos);
        }
        return result;
    }//author's blog == http://www.cnblogs.com/toulanboy/
};
```


## 运行结果

```
Runtime: 272 ms, faster than 16.30% of C++ online submissions for Merge k Sorted Lists.
Memory Usage: 10.8 MB, less than 93.58% of C++ online submissions for Merge k Sorted Lists.
```

